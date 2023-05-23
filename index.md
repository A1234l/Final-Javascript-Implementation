<html>
    <head>
        <!-- load jQuery and tablesorter scripts -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.10.25/css/jquery.dataTables.min.css">
    <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.10.25/js/jquery.dataTables.min.js"></script>
        <style>
            /* CSS-style selector maps to table id or other id's in HTML */
            #jsonTable{
                background-color: #353b45;
                padding: 10px;
                border: 3px solid #ccc;
                box-shadow: 0.8em 0.4em 0.4em grey;
                width: 100%;
            }
            #jsonRow tr{
                background-color: #353b45;
                padding: 10px;
                border: 3px solid #ccc;
                box-shadow: 0.8em 0.4em 0.4em grey;
                width: 100%;
            }
            #jsonHead tr th{
                color: #f7f7f7;
            }
            #jsonTable, #jsonRow tr th{
                color: #f7f7f7;
            }
        </style>
    </head>
    <body>
        <table id="jsonTable">
            <thead id="jsonHead">
                <tr>
                    <th>Compound Name</th>
                    <th>Mass in g/mol</th>
                    <th>Hazards</th>
                    <th>Other Properties</th>
                    <th>Molecular Structure</th>
                </tr>
            </thead>
            <tbody id="jsonRow"></tbody>
        </table>
    </body>
</html>

<script>
class Compound {
  constructor(name, mass, hazard, properties, structure) {
    this.name = name;
    this.mass = mass;
    this.hazard = hazard;
    this.properties = properties;
    this.structure = structure;
  }

  get() {
    const obj = {name: this.name, mass: this.mass, hazard: this.hazard, properties: this.properties, structure: this.structure};
    const json = JSON.stringify(obj);
    return json;
  }
}

class Compoundcollection {
  constructor(compound, group) {
    this.compounds = [...compound]; // ... spread option
    this.json = '{' + group + "_compounds:" + '[' + this.compounds.map(compound => compound.get()) + ']}';
  }

  displayCollection() {
    element.append("<br>Compound Collection object in JSON<br>");
    element.append(this.json + "<br>");  
    //alert(this.json);
  }
}

function constructBenzenes() {
    // define a benzylic Array of Compound objects
    const benzylic = [ 
        new Compound("Benzene", "78.11", "Flammable, Irritant, Health Hazard", "Colorless liquid, sweet odor, slightly soluble in water", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=241&t=s"),
        new Compound("Hydroquinone", "110.11", "Corrosive, Irritant, Health Hazard, Environmental Hazard", "Forms light colored crystals or solutions, used to treat skin discoloration", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=785&t=s"),
        new Compound("o-Xylene", "106.16", "Flammable, Irritant, Health Hazard", "Colorless watery liquid, sweet odor, insoluble in water", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=7237&t=s"),
        new Compound("Phenol", "94.11", "Corrosive, Acute Toxic, Health Hazard", "Colorless-to-white solid when pure, sickingly sweet and tarry odor, can catch fire", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=996&t=s"),
        new Compound("4-tert-Amylphenol", "164.24", "Corrosive, Irritant, Environmental Hazard", "Looks like colorless needles or beige solid", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=6643&t=s"),
        new Compound("Benzoic Acid", "122.12", "Corrosive, Health Hazard", "Looks like a white crystalline solid, slightly soluble in water, can harm the environment","https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=243&t=s")
    ];

    console.log(new Compoundcollection(benzylic, "Benzylic"));
    // make a collection of compounds
    return benzylic;
}

function constructCyclic() {
    // define a cyclo Array of Compound objects
    const cyclo = [ 
        new Compound("Cyclohexane", "84.16", "Flammable, Irritant, Health Hazard, Environmental Hazard", "Colorless liquid, petroleum-like odor, used to make nylon, paint remover, etc.", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=8078&t=s"),
        new Compound("Cyclohexylamine", "99.17", "Corrosive, Irritant, Health Hazard", "Clear, colorless to yellow liquid, smells like ammonia", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=7965&t=s"),
        new Compound("Cyclopentanone", "84.12", "Flammable, Irritant", "Colorless liquid, petroleum-like odor, insoluble in water", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=8452&t=s"),
        new Compound("Cyclohexyl methacrylate", "168.23", "Irritant", "Colorless liquid with pleasant odor, insoluble in water", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=7561&t=s"),
        new Compound("Cycloleucine", "129.16", "Acute Toxic", "EC 2.5.1.6 inhibitor, formed through cyclization of leucine", "https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=2901&t=s"),
        new Compound("2,5-Piperazinedione", "114.10", "Irritant", "Cyclic peptide, naturally found in Aspergillus fumigatus","https://pubchem.ncbi.nlm.nih.gov/image/imgsrv.fcgi?cid=7817&t=s")
    ];

    console.log(new Compoundcollection(cyclo, "Cyclic"));

    // make a collection of compounds
    return cyclo;
}

const benzeneDatabase = constructBenzenes();
const cyclicDatabase = constructCyclic();

for(var row of benzeneDatabase){
    $('#jsonRow').append('<tr><td>' + 
    row.name + '</td><td>' + 
    row.mass + '</td><td>' + 
    row.hazard + '</td><td>' +
    row.properties + '</td><td><img src=' + 
    row.structure + '></td></tr>');
    };
for(var row of cyclicDatabase){
    $('#jsonRow').append('<tr><td>' + 
    row.name + '</td><td>' + 
    row.mass + '</td><td>' + 
    row.hazard + '</td><td>' +
    row.properties + '</td><td><img src=' + 
    row.structure + '></td></tr>');
    };
$("#jsonTable").DataTable();
</script>