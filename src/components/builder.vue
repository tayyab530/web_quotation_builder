<!-- <div class="page-container">
        <main class="main-content">
            <div id="invoice-app">
                <div class="header">
                    <div>
                        <h1>Invoice Builder</h1>
                        <p>Date:  <input type="date" v-model="invoiceDate"></p>
                    </div>
                    <div>
                        <div class="section-spacer">
                            <input type="text" placeholder="Company Name" class="company-name" v-model="company.name">
                            <textarea v-on:keyup="adjustTextAreaHeight" v-model="company.contact"></textarea>
                        </div>
                        <div class="section-spacer">
                            <p><strong>Bill to:</strong></p>
                            <textarea v-on:keyup="adjustTextAreaHeight" v-model="client"></textarea>
                        </div>
                    </div>
                </div>
                <div>
                    <label for="currency-picker">Currency:</label>
                    <select id="currency-picker" v-model="invoiceCurrency">
                        <option v-for="currency in currencies" :value="currency">{{ currency.code }} - {{ currency.name }}</option>
                    </select>
                </div>
                <table class="responsive-table">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Item</th>
                            <th>Price/unite</th>
                            <th>Quantity</th>
                            <th>Total</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tr v-for="(item, index) in items">
                        <td data-label="No">{{ index + 1 }}</td>
                        <td data-label="Item"><input type="text" v-model="item.description" /></td>
                        <td data-label="Price/unite"><div class="cell-with-input">{{ invoiceCurrency.symbol }} <input type="number" min="0" v-model="item.price" /></div></td>
                        <td data-label="Quantity"><input type="number" min="0" v-model="item.quantity" /></td>
                        <td data-label="Total">{{ decimalDigits(item.price * item.quantity) }}</td>
                        <td class="text-right"><button class="is-danger" v-on:click="deleteItem(index)">Delete item</button></td>
                    </tr>
                </table>
                <button v-on:click="addNewItem">Add item</button>
                <table>
                    <tr>
                        <td>Subtotal</td>
                        <td>{{ decimalDigits(subTotal) }}</td>
                    </tr>
                    <tr>
                        <td><div class="cell-with-input">Discount <input class="text-right" type="number" min="0" max="100" v-model="discountRate" />%</div></td>
                        <td>{{ decimalDigits(discountTotal) }}</td>
                    </tr>
                    <tr>
                        <td><div class="cell-with-input">Tax <input class="text-right" type="number" min="0" max="100" v-model="taxRate" />%</div></td>
                        <td>{{ decimalDigits(taxTotal) }}</td>
                    </tr>
                    <tr class="text-bold">
                        <td>Grand Total</td>
                        <td>{{ decimalDigits(grandTotal) }}</td>
                    </tr>
                </table>
                <div class="section-spacer">
                    <p>Notes:</p>
                    <textarea v-on:keyup="adjustTextAreaHeight"></textarea>
                </div>
                
                <div class="section-spacer">
                    <p>Terms:</p>
                    <textarea v-on:keyup="adjustTextAreaHeight"></textarea>
                </div>
                <button v-on:click="printInvoice">Print Invoice</button>
            </div>
            
        </main>
</div>

document.addEventListener('DOMContentLoaded', function(){ 
    var app = new Vue({
        el: '#invoice-app',
        data: {
            invoiceCurrency: {
                "symbol": "$",
                "name": "US Dollar",
                "symbol_native": "$",
                "decimal_digits": 2,
                "rounding": 0,
                "code": "USD",
                "name_plural": "US dollars"
            },
            taxRate: 20,
            discountRate: 0,
            items: [
                { description: 'Item name', quantity: 0, price: 0 },
                { description: 'Item name', quantity: 0, price: 0 },

            ],
            currencies: currenciesData,
            company: {
                name: 'Your company name',
                contact: 'Your address\nYour tel\nYour email'
            },
            client: 'Client information',
            invoiceDate: ''
        },
        methods: {
            addNewItem: function() {
                this.items.push(
                    { description: 'Item name', quantity: 0, price: 0 }
                )
            },
            deleteItem: function(index) {
                this.items.splice(index, 1)
            },
            decimalDigits: function(value) {
                return this.invoiceCurrency.symbol + ' ' + value.toFixed(this.invoiceCurrency.decimal_digits);
            },
            printInvoice: function() {
                window.print();
            },
            adjustTextAreaHeight: function(event){
                var el = event.target;
                el.style.height = "1px";
                el.style.height = (25+el.scrollHeight)+"px";
            }
        },
        computed: {
            subTotal: function() {
                var total = this.items.reduce(function(accumulator, item) {
                    return accumulator + (item.price * item.quantity);
                }, 0)
                return total;
            },
            discountTotal: function() {
                var total = this.subTotal * (this.discountRate / 100);
                return total;
            },
            taxTotal: function() {
                var total = (this.subTotal - this.discountTotal) * (this.taxRate / 100);
                return total;
            },
            grandTotal: function() {
                var total = (this.subTotal - this.discountTotal) + this.taxTotal;
                return total;
            }
        }
    });
}, false);

var currenciesData=[{"symbol":"$","name":"US Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"USD","name_plural":"US dollars"},{"symbol":"CA$","name":"Canadian Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"CAD","name_plural":"Canadian dollars"},{"symbol":"???","name":"Euro","symbol_native":"???","decimal_digits":2,"rounding":0,"code":"EUR","name_plural":"euros"},{"symbol":"AED","name":"United Arab Emirates Dirham","symbol_native":"??.??.???","decimal_digits":2,"rounding":0,"code":"AED","name_plural":"UAE dirhams"},{"symbol":"Af","name":"Afghan Afghani","symbol_native":"??","decimal_digits":0,"rounding":0,"code":"AFN","name_plural":"Afghan Afghanis"},{"symbol":"ALL","name":"Albanian Lek","symbol_native":"Lek","decimal_digits":0,"rounding":0,"code":"ALL","name_plural":"Albanian lek??"},{"symbol":"AMD","name":"Armenian Dram","symbol_native":"????.","decimal_digits":0,"rounding":0,"code":"AMD","name_plural":"Armenian drams"},{"symbol":"AR$","name":"Argentine Peso","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"ARS","name_plural":"Argentine pesos"},{"symbol":"AU$","name":"Australian Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"AUD","name_plural":"Australian dollars"},{"symbol":"man.","name":"Azerbaijani Manat","symbol_native":"??????.","decimal_digits":2,"rounding":0,"code":"AZN","name_plural":"Azerbaijani manats"},{"symbol":"KM","name":"Bosnia-Herzegovina Convertible Mark","symbol_native":"KM","decimal_digits":2,"rounding":0,"code":"BAM","name_plural":"Bosnia-Herzegovina convertible marks"},{"symbol":"Tk","name":"Bangladeshi Taka","symbol_native":"???","decimal_digits":2,"rounding":0,"code":"BDT","name_plural":"Bangladeshi takas"},{"symbol":"BGN","name":"Bulgarian Lev","symbol_native":"????.","decimal_digits":2,"rounding":0,"code":"BGN","name_plural":"Bulgarian leva"},{"symbol":"BD","name":"Bahraini Dinar","symbol_native":"??.??.???","decimal_digits":3,"rounding":0,"code":"BHD","name_plural":"Bahraini dinars"},{"symbol":"FBu","name":"Burundian Franc","symbol_native":"FBu","decimal_digits":0,"rounding":0,"code":"BIF","name_plural":"Burundian francs"},{"symbol":"BN$","name":"Brunei Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"BND","name_plural":"Brunei dollars"},{"symbol":"Bs","name":"Bolivian Boliviano","symbol_native":"Bs","decimal_digits":2,"rounding":0,"code":"BOB","name_plural":"Bolivian bolivianos"},{"symbol":"R$","name":"Brazilian Real","symbol_native":"R$","decimal_digits":2,"rounding":0,"code":"BRL","name_plural":"Brazilian reals"},{"symbol":"BWP","name":"Botswanan Pula","symbol_native":"P","decimal_digits":2,"rounding":0,"code":"BWP","name_plural":"Botswanan pulas"},{"symbol":"BYR","name":"Belarusian Ruble","symbol_native":"BYR","decimal_digits":0,"rounding":0,"code":"BYR","name_plural":"Belarusian rubles"},{"symbol":"BZ$","name":"Belize Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"BZD","name_plural":"Belize dollars"},{"symbol":"CDF","name":"Congolese Franc","symbol_native":"FrCD","decimal_digits":2,"rounding":0,"code":"CDF","name_plural":"Congolese francs"},{"symbol":"CHF","name":"Swiss Franc","symbol_native":"CHF","decimal_digits":2,"rounding":0.05,"code":"CHF","name_plural":"Swiss francs"},{"symbol":"CL$","name":"Chilean Peso","symbol_native":"$","decimal_digits":0,"rounding":0,"code":"CLP","name_plural":"Chilean pesos"},{"symbol":"CN??","name":"Chinese Yuan","symbol_native":"CN??","decimal_digits":2,"rounding":0,"code":"CNY","name_plural":"Chinese yuan"},{"symbol":"CO$","name":"Colombian Peso","symbol_native":"$","decimal_digits":0,"rounding":0,"code":"COP","name_plural":"Colombian pesos"},{"symbol":"???","name":"Costa Rican Col??n","symbol_native":"???","decimal_digits":0,"rounding":0,"code":"CRC","name_plural":"Costa Rican col??ns"},{"symbol":"CV$","name":"Cape Verdean Escudo","symbol_native":"CV$","decimal_digits":2,"rounding":0,"code":"CVE","name_plural":"Cape Verdean escudos"},{"symbol":"K??","name":"Czech Republic Koruna","symbol_native":"K??","decimal_digits":2,"rounding":0,"code":"CZK","name_plural":"Czech Republic korunas"},{"symbol":"Fdj","name":"Djiboutian Franc","symbol_native":"Fdj","decimal_digits":0,"rounding":0,"code":"DJF","name_plural":"Djiboutian francs"},{"symbol":"Dkr","name":"Danish Krone","symbol_native":"kr","decimal_digits":2,"rounding":0,"code":"DKK","name_plural":"Danish kroner"},{"symbol":"RD$","name":"Dominican Peso","symbol_native":"RD$","decimal_digits":2,"rounding":0,"code":"DOP","name_plural":"Dominican pesos"},{"symbol":"DA","name":"Algerian Dinar","symbol_native":"??.??.???","decimal_digits":2,"rounding":0,"code":"DZD","name_plural":"Algerian dinars"},{"symbol":"Ekr","name":"Estonian Kroon","symbol_native":"kr","decimal_digits":2,"rounding":0,"code":"EEK","name_plural":"Estonian kroons"},{"symbol":"EGP","name":"Egyptian Pound","symbol_native":"??.??.???","decimal_digits":2,"rounding":0,"code":"EGP","name_plural":"Egyptian pounds"},{"symbol":"Nfk","name":"Eritrean Nakfa","symbol_native":"Nfk","decimal_digits":2,"rounding":0,"code":"ERN","name_plural":"Eritrean nakfas"},{"symbol":"Br","name":"Ethiopian Birr","symbol_native":"Br","decimal_digits":2,"rounding":0,"code":"ETB","name_plural":"Ethiopian birrs"},{"symbol":"??","name":"British Pound Sterling","symbol_native":"??","decimal_digits":2,"rounding":0,"code":"GBP","name_plural":"British pounds sterling"},{"symbol":"GEL","name":"Georgian Lari","symbol_native":"GEL","decimal_digits":2,"rounding":0,"code":"GEL","name_plural":"Georgian laris"},{"symbol":"GH???","name":"Ghanaian Cedi","symbol_native":"GH???","decimal_digits":2,"rounding":0,"code":"GHS","name_plural":"Ghanaian cedis"},{"symbol":"FG","name":"Guinean Franc","symbol_native":"FG","decimal_digits":0,"rounding":0,"code":"GNF","name_plural":"Guinean francs"},{"symbol":"GTQ","name":"Guatemalan Quetzal","symbol_native":"Q","decimal_digits":2,"rounding":0,"code":"GTQ","name_plural":"Guatemalan quetzals"},{"symbol":"HK$","name":"Hong Kong Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"HKD","name_plural":"Hong Kong dollars"},{"symbol":"HNL","name":"Honduran Lempira","symbol_native":"L","decimal_digits":2,"rounding":0,"code":"HNL","name_plural":"Honduran lempiras"},{"symbol":"kn","name":"Croatian Kuna","symbol_native":"kn","decimal_digits":2,"rounding":0,"code":"HRK","name_plural":"Croatian kunas"},{"symbol":"Ft","name":"Hungarian Forint","symbol_native":"Ft","decimal_digits":0,"rounding":0,"code":"HUF","name_plural":"Hungarian forints"},{"symbol":"Rp","name":"Indonesian Rupiah","symbol_native":"Rp","decimal_digits":0,"rounding":0,"code":"IDR","name_plural":"Indonesian rupiahs"},{"symbol":"???","name":"Israeli New Sheqel","symbol_native":"???","decimal_digits":2,"rounding":0,"code":"ILS","name_plural":"Israeli new sheqels"},{"symbol":"Rs","name":"Indian Rupee","symbol_native":"?????????","decimal_digits":2,"rounding":0,"code":"INR","name_plural":"Indian rupees"},{"symbol":"IQD","name":"Iraqi Dinar","symbol_native":"??.??.???","decimal_digits":0,"rounding":0,"code":"IQD","name_plural":"Iraqi dinars"},{"symbol":"IRR","name":"Iranian Rial","symbol_native":"???","decimal_digits":0,"rounding":0,"code":"IRR","name_plural":"Iranian rials"},{"symbol":"Ikr","name":"Icelandic Kr??na","symbol_native":"kr","decimal_digits":0,"rounding":0,"code":"ISK","name_plural":"Icelandic kr??nur"},{"symbol":"J$","name":"Jamaican Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"JMD","name_plural":"Jamaican dollars"},{"symbol":"JD","name":"Jordanian Dinar","symbol_native":"??.??.???","decimal_digits":3,"rounding":0,"code":"JOD","name_plural":"Jordanian dinars"},{"symbol":"??","name":"Japanese Yen","symbol_native":"???","decimal_digits":0,"rounding":0,"code":"JPY","name_plural":"Japanese yen"},{"symbol":"Ksh","name":"Kenyan Shilling","symbol_native":"Ksh","decimal_digits":2,"rounding":0,"code":"KES","name_plural":"Kenyan shillings"},{"symbol":"KHR","name":"Cambodian Riel","symbol_native":"???","decimal_digits":2,"rounding":0,"code":"KHR","name_plural":"Cambodian riels"},{"symbol":"CF","name":"Comorian Franc","symbol_native":"FC","decimal_digits":0,"rounding":0,"code":"KMF","name_plural":"Comorian francs"},{"symbol":"???","name":"South Korean Won","symbol_native":"???","decimal_digits":0,"rounding":0,"code":"KRW","name_plural":"South Korean won"},{"symbol":"KD","name":"Kuwaiti Dinar","symbol_native":"??.??.???","decimal_digits":3,"rounding":0,"code":"KWD","name_plural":"Kuwaiti dinars"},{"symbol":"KZT","name":"Kazakhstani Tenge","symbol_native":"??????.","decimal_digits":2,"rounding":0,"code":"KZT","name_plural":"Kazakhstani tenges"},{"symbol":"LB??","name":"Lebanese Pound","symbol_native":"??.??.???","decimal_digits":0,"rounding":0,"code":"LBP","name_plural":"Lebanese pounds"},{"symbol":"SLRs","name":"Sri Lankan Rupee","symbol_native":"SL Re","decimal_digits":2,"rounding":0,"code":"LKR","name_plural":"Sri Lankan rupees"},{"symbol":"Lt","name":"Lithuanian Litas","symbol_native":"Lt","decimal_digits":2,"rounding":0,"code":"LTL","name_plural":"Lithuanian litai"},{"symbol":"Ls","name":"Latvian Lats","symbol_native":"Ls","decimal_digits":2,"rounding":0,"code":"LVL","name_plural":"Latvian lati"},{"symbol":"LD","name":"Libyan Dinar","symbol_native":"??.??.???","decimal_digits":3,"rounding":0,"code":"LYD","name_plural":"Libyan dinars"},{"symbol":"MAD","name":"Moroccan Dirham","symbol_native":"??.??.???","decimal_digits":2,"rounding":0,"code":"MAD","name_plural":"Moroccan dirhams"},{"symbol":"MDL","name":"Moldovan Leu","symbol_native":"MDL","decimal_digits":2,"rounding":0,"code":"MDL","name_plural":"Moldovan lei"},{"symbol":"MGA","name":"Malagasy Ariary","symbol_native":"MGA","decimal_digits":0,"rounding":0,"code":"MGA","name_plural":"Malagasy Ariaries"},{"symbol":"MKD","name":"Macedonian Denar","symbol_native":"MKD","decimal_digits":2,"rounding":0,"code":"MKD","name_plural":"Macedonian denari"},{"symbol":"MMK","name":"Myanma Kyat","symbol_native":"K","decimal_digits":0,"rounding":0,"code":"MMK","name_plural":"Myanma kyats"},{"symbol":"MOP$","name":"Macanese Pataca","symbol_native":"MOP$","decimal_digits":2,"rounding":0,"code":"MOP","name_plural":"Macanese patacas"},{"symbol":"MURs","name":"Mauritian Rupee","symbol_native":"MURs","decimal_digits":0,"rounding":0,"code":"MUR","name_plural":"Mauritian rupees"},{"symbol":"MX$","name":"Mexican Peso","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"MXN","name_plural":"Mexican pesos"},{"symbol":"RM","name":"Malaysian Ringgit","symbol_native":"RM","decimal_digits":2,"rounding":0,"code":"MYR","name_plural":"Malaysian ringgits"},{"symbol":"MTn","name":"Mozambican Metical","symbol_native":"MTn","decimal_digits":2,"rounding":0,"code":"MZN","name_plural":"Mozambican meticals"},{"symbol":"N$","name":"Namibian Dollar","symbol_native":"N$","decimal_digits":2,"rounding":0,"code":"NAD","name_plural":"Namibian dollars"},{"symbol":"???","name":"Nigerian Naira","symbol_native":"???","decimal_digits":2,"rounding":0,"code":"NGN","name_plural":"Nigerian nairas"},{"symbol":"C$","name":"Nicaraguan C??rdoba","symbol_native":"C$","decimal_digits":2,"rounding":0,"code":"NIO","name_plural":"Nicaraguan c??rdobas"},{"symbol":"Nkr","name":"Norwegian Krone","symbol_native":"kr","decimal_digits":2,"rounding":0,"code":"NOK","name_plural":"Norwegian kroner"},{"symbol":"NPRs","name":"Nepalese Rupee","symbol_native":"????????????","decimal_digits":2,"rounding":0,"code":"NPR","name_plural":"Nepalese rupees"},{"symbol":"NZ$","name":"New Zealand Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"NZD","name_plural":"New Zealand dollars"},{"symbol":"OMR","name":"Omani Rial","symbol_native":"??.??.???","decimal_digits":3,"rounding":0,"code":"OMR","name_plural":"Omani rials"},{"symbol":"B/.","name":"Panamanian Balboa","symbol_native":"B/.","decimal_digits":2,"rounding":0,"code":"PAB","name_plural":"Panamanian balboas"},{"symbol":"S/.","name":"Peruvian Nuevo Sol","symbol_native":"S/.","decimal_digits":2,"rounding":0,"code":"PEN","name_plural":"Peruvian nuevos soles"},{"symbol":"???","name":"Philippine Peso","symbol_native":"???","decimal_digits":2,"rounding":0,"code":"PHP","name_plural":"Philippine pesos"},{"symbol":"PKRs","name":"Pakistani Rupee","symbol_native":"???","decimal_digits":0,"rounding":0,"code":"PKR","name_plural":"Pakistani rupees"},{"symbol":"z??","name":"Polish Zloty","symbol_native":"z??","decimal_digits":2,"rounding":0,"code":"PLN","name_plural":"Polish zlotys"},{"symbol":"???","name":"Paraguayan Guarani","symbol_native":"???","decimal_digits":0,"rounding":0,"code":"PYG","name_plural":"Paraguayan guaranis"},{"symbol":"QR","name":"Qatari Rial","symbol_native":"??.??.???","decimal_digits":2,"rounding":0,"code":"QAR","name_plural":"Qatari rials"},{"symbol":"RON","name":"Romanian Leu","symbol_native":"RON","decimal_digits":2,"rounding":0,"code":"RON","name_plural":"Romanian lei"},{"symbol":"din.","name":"Serbian Dinar","symbol_native":"??????.","decimal_digits":0,"rounding":0,"code":"RSD","name_plural":"Serbian dinars"},{"symbol":"RUB","name":"Russian Ruble","symbol_native":"??????.","decimal_digits":2,"rounding":0,"code":"RUB","name_plural":"Russian rubles"},{"symbol":"RWF","name":"Rwandan Franc","symbol_native":"FR","decimal_digits":0,"rounding":0,"code":"RWF","name_plural":"Rwandan francs"},{"symbol":"SR","name":"Saudi Riyal","symbol_native":"??.??.???","decimal_digits":2,"rounding":0,"code":"SAR","name_plural":"Saudi riyals"},{"symbol":"SDG","name":"Sudanese Pound","symbol_native":"SDG","decimal_digits":2,"rounding":0,"code":"SDG","name_plural":"Sudanese pounds"},{"symbol":"Skr","name":"Swedish Krona","symbol_native":"kr","decimal_digits":2,"rounding":0,"code":"SEK","name_plural":"Swedish kronor"},{"symbol":"S$","name":"Singapore Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"SGD","name_plural":"Singapore dollars"},{"symbol":"Ssh","name":"Somali Shilling","symbol_native":"Ssh","decimal_digits":0,"rounding":0,"code":"SOS","name_plural":"Somali shillings"},{"symbol":"SY??","name":"Syrian Pound","symbol_native":"??.??.???","decimal_digits":0,"rounding":0,"code":"SYP","name_plural":"Syrian pounds"},{"symbol":"???","name":"Thai Baht","symbol_native":"???","decimal_digits":2,"rounding":0,"code":"THB","name_plural":"Thai baht"},{"symbol":"DT","name":"Tunisian Dinar","symbol_native":"??.??.???","decimal_digits":3,"rounding":0,"code":"TND","name_plural":"Tunisian dinars"},{"symbol":"T$","name":"Tongan Pa??anga","symbol_native":"T$","decimal_digits":2,"rounding":0,"code":"TOP","name_plural":"Tongan pa??anga"},{"symbol":"TL","name":"Turkish Lira","symbol_native":"TL","decimal_digits":2,"rounding":0,"code":"TRY","name_plural":"Turkish Lira"},{"symbol":"TT$","name":"Trinidad and Tobago Dollar","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"TTD","name_plural":"Trinidad and Tobago dollars"},{"symbol":"NT$","name":"New Taiwan Dollar","symbol_native":"NT$","decimal_digits":2,"rounding":0,"code":"TWD","name_plural":"New Taiwan dollars"},{"symbol":"TSh","name":"Tanzanian Shilling","symbol_native":"TSh","decimal_digits":0,"rounding":0,"code":"TZS","name_plural":"Tanzanian shillings"},{"symbol":"???","name":"Ukrainian Hryvnia","symbol_native":"???","decimal_digits":2,"rounding":0,"code":"UAH","name_plural":"Ukrainian hryvnias"},{"symbol":"USh","name":"Ugandan Shilling","symbol_native":"USh","decimal_digits":0,"rounding":0,"code":"UGX","name_plural":"Ugandan shillings"},{"symbol":"$U","name":"Uruguayan Peso","symbol_native":"$","decimal_digits":2,"rounding":0,"code":"UYU","name_plural":"Uruguayan pesos"},{"symbol":"UZS","name":"Uzbekistan Som","symbol_native":"UZS","decimal_digits":0,"rounding":0,"code":"UZS","name_plural":"Uzbekistan som"},{"symbol":"Bs.F.","name":"Venezuelan Bol??var","symbol_native":"Bs.F.","decimal_digits":2,"rounding":0,"code":"VEF","name_plural":"Venezuelan bol??vars"},{"symbol":"???","name":"Vietnamese Dong","symbol_native":"???","decimal_digits":0,"rounding":0,"code":"VND","name_plural":"Vietnamese dong"},{"symbol":"FCFA","name":"CFA Franc BEAC","symbol_native":"FCFA","decimal_digits":0,"rounding":0,"code":"XAF","name_plural":"CFA francs BEAC"},{"symbol":"CFA","name":"CFA Franc BCEAO","symbol_native":"CFA","decimal_digits":0,"rounding":0,"code":"XOF","name_plural":"CFA francs BCEAO"},{"symbol":"YR","name":"Yemeni Rial","symbol_native":"??.??.???","decimal_digits":0,"rounding":0,"code":"YER","name_plural":"Yemeni rials"},{"symbol":"R","name":"South African Rand","symbol_native":"R","decimal_digits":2,"rounding":0,"code":"ZAR","name_plural":"South African rand"},{"symbol":"ZK","name":"Zambian Kwacha","symbol_native":"ZK","decimal_digits":0,"rounding":0,"code":"ZMK","name_plural":"Zambian kwachas"}];


$red: #ff5f6d;
$yellow: #ffc371;
$green: #81cf71;
$white: #faebd7;
$grey:  darken($white, 10%);

body {
    background: linear-gradient(to right, $red, $yellow);
}

.main-content {
    min-height: 100vh;
    padding: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    @media print {
        align-items: flex-start;
    }
}


#invoice-app {
    background-color: $white;
    padding: 2rem;
    border-radius: 0.5rem;
}

.header {
    @media (min-width: 761px) {
        display: flex;
    }

    > div {
        &:nth-child(-n+1){
            @media (min-width: 761px) {
                order: 1;
                flex: 1;
                text-align: right;
                padding-left: 1rem;
            }
        }
    }
}

.section-spacer {
    margin: 1rem 0;
}

input, select, textarea {
    background-color: transparentize($color: white, $amount: 0.7);
    border: none;
    display: inline-block;
    transition: background-color 0.3s ease-in-out;
    width: 100%;

    &:focus {
        outline-color: $yellow;
        background-color: transparentize($color: white, $amount: 0.4);
    }

    &:hover {
        background-color: transparentize($color: white, $amount: 0.5);
    }

    @media print {
        background-color: transparent;
    }

    @media only screen and (min-width: 761px) {
        width: auto;
    }
}

textarea {
    width: 100%;
    min-height: 80px;
}

select {
    @media 
    only screen and (max-width: 760px) {
        width: 100%;
    }

    @media print {
        appearance: none;
    }
}

.company-name {
    font-size: 2rem;
}

table{
    width: auto; 
    border-collapse: collapse; 
    margin: 2rem 0;
    
    thead{
        th {
            padding: 0.5rem 1rem;

            &:nth-child(-n+1){
                padding-left: 0;
            }
            &:nth-last-child(-n+1){
                padding-right: 0;
            }
        }
    }
    
    tr { 
        border-bottom: 1px solid $grey;

        td {
            padding: 0.5rem 1rem;

            &:nth-child(-n+1){
                padding-left: 0;
            }
            &:nth-last-child(-n+1){
                padding-right: 0;
            }

            input {
                width: 100%;
            }

            .cell-with-input {
                display: flex;

                input {
                    margin: 0 0.2rem;
                }
            }
        }
    }
}

.responsive-table {
    width: 100%;
    @media 
    only screen and (max-width: 760px) {

        table, thead, tbody, th, td, tr { 
            display: block; 
        }

        thead tr { 
            position: absolute;
            top: -9999px;
            left: -9999px;
        }

        tr {
            padding: 2rem 0;
        }

        
        td[data-label] {
            position: relative;
            padding-left: 40%; 
            display: flex;
            align-items: center;

            &:before { 
                content: attr(data-label);
                position: absolute;
                top: 0.5rem;
                left: 0;
                width: 35%; 
                padding-right: 10px; 
                white-space: nowrap;
                font-weight: bold;
            }
        }
    }
}

button {
    background-color: $green;
    border: none;
    border-radius: 100px;
    padding: 0.5rem 1rem;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;

    &:focus {
        outline-color: $yellow;
        background-color: darken($color: $green, $amount: 7%);
    }

    &:hover {
        background-color: darken($color: $green, $amount: 5%);
    }

    @media print {
        display: none;
    }

    &.is-danger{
        background-color: $red;

        &:focus {
            background-color: darken($color: $red, $amount: 7%);
        }
    
        &:hover {
            background-color: darken($color: $red, $amount: 5%);
        }
    }
}

.text-right {
    text-align: right;
}

.text-bold {
    font-weight: bold;
} -->