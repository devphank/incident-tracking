# incident-tracking
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Incident Status Tracking</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 20px;
            background-color: #f4f4f4;
        }
        .container {
            max-width: 700px;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        label {
            font-weight: bold;
            display: block;
            margin-top: 10px;
        }
        input, textarea {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        .status-group {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 5px;
        }
        .status-group label {
            font-weight: normal;
        }
        button {
            margin-top: 20px;
            padding: 10px;
            background: #007bff;
            color: white;
            border: none;
            cursor: pointer;
            width: 100%;
        }
        button:hover {
            background: #0056b3;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Incident Status Tracking</h2>
    
    <label for="unit">ST/Unit:</label>
    <input type="text" id="unit" placeholder="Enter Unit">
    
    <label for="name">Name:</label>
    <input type="text" id="name" placeholder="First Last">
    
    <label for="position">Position/Title:</label>
    <input type="text" id="position" placeholder="Section/Unit">
    
    <h3>Incident Entries</h3>
    
    <div id="incidentEntries">
        <!-- Incident entry template will be added dynamically -->
    </div>
    
    <button type="button" onclick="addIncidentEntry()">Add Incident Entry</button>
    <button type="button" onclick="submitForm()">Submit</button>
</div>

<script>
    function addIncidentEntry() {
        const container = document.getElementById("incidentEntries");
        const incidentIndex = container.childElementCount + 1;
        
        const entryHTML = `
            <div class="incident-entry">
                <h4>Incident ${incidentIndex}</h4>
                <label for="incidentLocation${incidentIndex}">Incident Location:</label>
                <input type="text" id="incidentLocation${incidentIndex}" placeholder="Enter Location">
                
                <label for="incidentTime${incidentIndex}">Time:</label>
                <input type="time" id="incidentTime${incidentIndex}">
                
                <label>Status:</label>
                <div class="status-group">
                    <label><input type="checkbox" id="assigned${incidentIndex}"> Assigned</label>
                    <label><input type="checkbox" id="available${incidentIndex}"> Available</label>
                    <label><input type="checkbox" id="osRest${incidentIndex}"> O/S Rest</label>
                    <label><input type="checkbox" id="osPers${incidentIndex}"> O/S Pers</label>
                    <label><input type="checkbox" id="osMech${incidentIndex}"> O/S Mech</label>
                </div>
                
                <label for="etr${incidentIndex}">ETR:</label>
                <input type="text" id="etr${incidentIndex}" placeholder="Enter ETR">
                
                <label for="notes${incidentIndex}">Notes:</label>
                <textarea id="notes${incidentIndex}" rows="2" placeholder="Additional details..."></textarea>
                
                <hr>
            </div>
        `;
        
        container.insertAdjacentHTML("beforeend", entryHTML);
    }

    function submitForm() {
        let unit = document.getElementById("unit").value;
        let name = document.getElementById("name").value;
        let position = document.getElementById("position").value;

        if (!unit || !name || !position) {
            alert("Please fill out all required fields.");
            return;
        }

        let incidents = [];
        document.querySelectorAll(".incident-entry").forEach((entry, index) => {
            let location = document.getElementById(`incidentLocation${index + 1}`).value;
            let time = document.getElementById(`incidentTime${index + 1}`).value;
            let assigned = document.getElementById(`assigned${index + 1}`).checked;
            let available = document.getElementById(`available${index + 1}`).checked;
            let osRest = document.getElementById(`osRest${index + 1}`).checked;
            let osPers = document.getElementById(`osPers${index + 1}`).checked;
            let osMech = document.getElementById(`osMech${index + 1}`).checked;
            let etr = document.getElementById(`etr${index + 1}`).value;
            let notes = document.getElementById(`notes${index + 1}`).value;

            incidents.push({
                Location: location,
                Time: time,
                Status: {
                    Assigned: assigned,
                    Available: available,
                    "O/S Rest": osRest,
                    "O/S Pers": osPers,
                    "O/S Mech": osMech
                },
                ETR: etr,
                Notes: notes
            });
        });

        let formData = {
            Unit: unit,
            Name: name,
            Position: position,
            Incidents: incidents
        };

        console.log("Form Submitted", formData);
        alert("Form submitted successfully!");
    }
</script>

</body>
</html>
