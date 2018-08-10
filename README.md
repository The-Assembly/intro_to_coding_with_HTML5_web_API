# Intro to coding with HTML5 WebAPIs

WebAPI is a term used to refer to a suite of device compatibility and access APIs that allow Web apps and content to access device hardware (such as battery status or the device vibration hardware), as well as access to data stored on the device (such as the calendar or contacts list). By adding these APIs, we hope to expand what the Web can do today to also include what only proprietary platforms were able to do in the past.

**More on WebAPIs can be found on** https://developer.mozilla.org/en-US/docs/WebAPI

## Demonstrated WebAPIs
1. [Battery Status](#battery-status)
2. [Notification](#notification)
3. [Geolocation](#geolocation)
4. [Page Visibility](#page-visibility)
5. [Screen Orientation & Vibration (Mobile Only)](#screen-orientation-vibration-mobile-only)
<<<<<<< HEAD
7. [Clipboard & IndexedDB/WebSQL/LocalStorage](#clipboard-&-indexeddb-websql-localstorage)
=======
7. [Clipboard & IndexedDB/WebSQL/LocalStorage](#clipboard-indexeddb-websql-localstorage)
>>>>>>> 81984af1d5fe51da76518f22047bb619478317ce
9. [Device Motion (Using p5.js)](#device-motion-using-p5js)

## Battery Status
The **Battery Status API** provides information about the system's battery charge level and lets you be notified by events that are sent when the battery level or charging status change. This can be used to adjust your app's resource usage to reduce battery drain when the battery is low, or to save changes before the battery runs out in order to prevent data loss.

### Demo
The following demo shows how developers can access a user's battery information and show the battery level, the battery discharging time and if the device is charging or not.
```html
<!DOCTYPE html>
<html>
	<head>
		<title>Battery Status</title>
	</head>

	<body>
		<h2>What's my battery level?</h2>
		<h3 id="battery_level"></h3>
		<br>

		<h2>How long before my battery runs out?</h2>
		<h3 id="battery_discharge"></h3>
		<br>
	
		<h2>Is my laptop charging?</h2>
		<h3 id="battery_charging"></h3>
	</body>

	<script>
		navigator.getBattery().then(function(battery){
			// Displays the current battery level
			document.getElementById('battery_level').innerHTML = battery.level * 100 + "%"; // Get value in percentage

			// Displays time left before the battery runs out
			document.getElementById('battery_discharge').innerHTML = battery.dischargingTime / 60 / 60; // Get time in hours

			// Displays if battery is charging (Yes!) or not (No)
			document.getElementById('battery_charging').innerHTML = (battery.charging ==  true) ? 'Yes!' : 'No';
		});
	</script>
</html>

```
For reference, [visit this link.](https://developer.mozilla.org/en-US/docs/Web/API/Battery_Status_API)

## Notification
The **Notifications API** lets a web page or app send notifications that are displayed outside the page at the system level; this lets web apps send information to a user even if the application is idle or in the background.

### Demo
The following code demonstrates how a developer can get permission from the users to send notifications and send a simple notification.
```html
<html>
	<head>
		<title>Notification</title>
	</head>
	<body>
		<!-- Paragraph to display notification permission status -->
		<p id="perm"></p>
		
		<!-- Button to trigger the notification event -->
		<button onclick="notify()">Notify</button>
	</body>
	<script>
		// Function that displays a notification on click of button
		function notify(){
			// Get permission from user to display the notification
			// through the browser
			Notification.requestPermission().then(function(result){
				// Display the permission status on the screen
				// 'granted', 'denied' or 'dismissed'
				document.getElementById('perm').innerHTML = result;

				// If the permission was granted, send a notification
				if(Notification.permission == "granted"){
					var notification = new Notification("Hey there!");
				}
			});
		}
	</script>
</html>
```

For reference, [visit this link.](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API/Using_the_Notifications_API)

## Geolocation
The **Geolocation API** allows the user to provide their location to web applications if they so desire.

### Demo
The following code gets the latitude and longitutde of the user's location and outputs it on the screen.
```html
<!DOCTYPE html>
<html>
	<head>
		<title>Geolocation</title>
	</head>
	<body>
		<!-- Paragraph to display the user's current location -->
		<p id="location"></p>
	</body>
	<script>
		// Get current position
		navigator.geolocation.getCurrentPosition(function(pos){
			// Display the latitude and longitude on the paragraph
			document.getElementById('location').innerHTML = pos.coords.latitude + ", " + pos.coords.longitude;
		});
		
		// Check for change in position
		id = navigator.geolocation.watchPosition(
					// On success
					function(pos){
						// Do something when the location is changed
						console.log('Changed');
					},
					
					// On error
					function(){
						console.log('Error');
					}
				);
	</script>
</html>

```
For reference, [visit this link.](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)

## Page Visibility
With tabbed browsing, there is a reasonable chance that any given webpage is in the background and thus not visible to the user. The Page Visibility API provides events you can watch for to know when a document becomes visible or hidden.

### Demo
Using the **Page Visibility API**, the following code shows how to perform actions when the user focuses out of the window or hides the window.
```html
<!DOCTYPE html>
<html>
	<head>
		<title>Page Visibility</title>
	</head>
	<body>
		
	</body>
	<script>
		// Event listener to detect change in tab visibility
		document.addEventListener('visibilitychange', function(){
			// Check if the tab has gone out of focus or is hidden
			if(document.hidden){
				// Tab is hidden
				document.title = "COME BACK!";	
			} else {
				// Tab is visible
				document.title = "Yay!";
			}
		});
	</script>
</html>
```
For reference, [visit this link.](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API)

## Screen Orientation & Vibration (Mobile Only)
The **Screen Orientation API** provides the orientation of the screen being used and the **Vibration API** offers web apps the ability to access the vibration hardware, which lets software code provide physical feedback to the user by causing the device to shake.

### Demo
The following code combines the two APIs (Screen Orientation and Vibration) to make the device when its tilted to the right and stop the vibration when it is tilted to the left.
```html
<!DOCTYPE html>
<html>
	<head>
		<title>Screen Orientation</title>
	</head>
	<body>
		<!-- To be able to access the vibration API, the user must explicitly
			 perform some event such as a button click. This is to make sure
			 that developers do not misuse the vibration API to make it vibrate
			 at any given time.
		-->

		<!-- The button below allows the vibration API to be triggered on button
			 click. Once the user allows this action, the developer can trigger
			 vibration events at any time until the page is refreshed or destroyed.
		-->
		<button onclick="navigator.vibrate(0);" id="btn">Allow Vibrate</button>

		<p id="orient"></p>
	</body>
	<script>
		// Add an event listener to check for a change in the orientation
		window.addEventListener('orientationchange', function(e){
			// Check if the device is tilted to the right
			// -90 -> Tilted to the right
			// 0 -> Straight/Portrait
			// 90 -> Tilted to left
			if(window.orientation == -90){
				// Display tilt side on webpage
				document.getElementById('orient').innerHTML = "Tilted Right";
				
				// Vibrate for 10 seconds
				navigator.vibrate(10000);
			} else if(window.orientation == 90){
				document.getElementById('orient').innerHTML = "Tilted Left";

				// Stop the vibration
				navigator.vibrate(0);
			}
		});
	</script>
</html>

```

For reference, visit [Screen Orientation API](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model/Managing_screen_orientation) and [Vibration API](https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API).

## Clipboard & IndexedDB/WebSQL/LocalStorage
The **Clipboard API** ([reference](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)) is a modern API providing asynchronous functions and secure context support allowing developers to access the clipboard.

**IndexedDB** ([reference](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)) is a low-level API for client-side storage of significant amounts of structured data, including files/blobs. This API uses indexes to enable high-performance searches of this data.

**WebSQL** ([reference](https://www.w3.org/TR/webdatabase/)) provides an SQL database on the client-side. Developers can use basic SQL transactions such as `SELECT`, `INSERT`, `ALTER`, etc., to access or modify the database contents. 

**LocalStorage** ([reference](https://developer.mozilla.org/en-US/docs/Web/API/Storage/LocalStorage)) is a lightweight way to store key-value pairs. The API is very simple, but usage is capped at 5MB in many browsers. 

### Demo
The codes below demonstrate how a user's copied text can be stored locally in client side database.
#### Using IndexedDB
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Clipboard & IndexedDB</title>
    </head>
    <body>
        <!-- Dummy paragraphs to be able to copy some text -->
        <p id="para1">1. This is paragraph 1</p>
        <button id="copy_btn" onclick="copy_store(1)">Copy</button>

        <p id="para2">2. This is paragraph 2</p>
        <button id="copy_btn" onclick="copy_store(2)">Copy</button>

        <p id="para3">3. This is paragraph 3</p>
        <button id="copy_btn" onclick="copy_store(3)">Copy</button>

        <p id="para4">4. This is paragraph 4</p>
        <button id="copy_btn" onclick="copy_store(4)">Copy</button>
        
        <div id="container"></div>
    </body>
    <script>
        // In the following line, you should include the prefixes of implementations you want to test.
        window.indexedDB = window.indexedDB || window.mozIndexedDB || window.webkitIndexedDB || window.msIndexedDB;
        // DON'T use "var indexedDB = ..." if you're not in a function.
        // Moreover, you may need references to some window.IDB* objects:
        window.IDBTransaction = window.IDBTransaction || window.webkitIDBTransaction || window.msIDBTransaction || {READ_WRITE: "readwrite"}; // This line should only be needed if it is needed to support the object's constants for older browsers
        window.IDBKeyRange = window.IDBKeyRange || window.webkitIDBKeyRange || window.msIDBKeyRange;

        // Check if IndexedDB is supported
        if (!window.indexedDB) {
            window.alert("Your browser doesn't support a stable version of IndexedDB. Such and such feature will not be available.");
        }

        // Open the database
        var db;
        
        // Name the database 'clipboard_db' and the version as 1
        request = window.indexedDB.open("clipboard_db", 1);

        data = [];

        // In case there was an error
        request.onerror = function(event) {
            alert("Error in creating database");
        };

        // If the operation was successful
        request.onsuccess = function(event) {
            db = event.target.result;

            // Call the function to retrieve information and display it
            get_data();
        };

        // This event is only implemented in recent browsers   
        request.onupgradeneeded = function(event) { 
            var db = event.target.result;

            // Create an objectStore to hold information about our clips.
            objectStore = db.createObjectStore("clips", { keyPath: "id" });
        };

        // Function to select a specific paragraph
        function select(para_num){
            // Create a Range object
            var range = document.createRange();

            // Gets the Selection object
            var selection = window.getSelection();

            // Specifies what is to be selected
            range.selectNodeContents(document.getElementById('para' + para_num));

            // Removes any previous selections
            selection.removeAllRanges();

            // Adds the selection to only the given range
            selection.addRange(range);
        }

        // Function to copy selected text and store information
        function copy_store(para_num){
            // Get text from <p> tag
            var text = document.getElementById('para' + para_num).innerHTML;

            // Store information on the data variable
            data.push({id: para_num, para: text});

            // Create an object store to hold your copied text and open it for read/write
            var dataStore = db.transaction("clips", "readwrite").objectStore("clips");
            
            // For each item in the data variable, add it to the database
            data.forEach(function(data) {
                dataStore.add(data);
            });
            
            // Select text for the specific paragraph
            select(para_num);

            try {
                // Trigger copy event
                document.execCommand('copy');
            } catch (err) {
                console.log('Error: ' + err);
            }
        }

        // Function to retrieve data from database and display it
        function get_data(){
            // Get a reference to the object store where our copied text is stored
            var objectStore = db.transaction(["clips"]).objectStore("clips");
            
            // Open cursor to iterate over the objects in the object store
            var request = objectStore.openCursor();

            // Get reference to the HTML element where data is displayed
            var container = document.getElementById('container');
            container.innerHTML = "<h1>Contents</h1>";
            
            // Handle errors in case object store couldn't be accessed
            request.onerror = function(event) {
                // Handle errors!
                console.log('Error accessing data');
            };
            
            // If the request is successful, output it on the screen
            request.onsuccess = function(event) {
                let cursor = event.target.result;
                
                // Get all records
                if (cursor) {
                    // Values on the database
                    let value = cursor.value;

                    // Display data in HTML element
                    // value.para -> para is the column name given in the object store
                    container.innerHTML += value.para + "<br>";

                    // Move the cursor to the next item if any
                    cursor.continue();
                }
            };
        }
    </script>
</html>
```
#### Using WebSQL
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Clipboard & WebSQL</title>
    </head>
    <body>
        <!-- Dummy paragraphs to be able to copy some text -->
        <p id="para1">1. This is paragraph 1</p>
        <button id="copy_btn" onclick="copy_store(1)">Copy</button>

        <p id="para2">2. This is paragraph 2</p>
        <button id="copy_btn" onclick="copy_store(2)">Copy</button>

        <p id="para3">3. This is paragraph 3</p>
        <button id="copy_btn" onclick="copy_store(3)">Copy</button>

        <p id="para4">4. This is paragraph 4</p>
        <button id="copy_btn" onclick="copy_store(4)">Copy</button>
        
        <div id="container"></div>
    </body>
    <script>
        // Open the database
        // Args list: openDatabase( db_name, version, description, size )
        var db = openDatabase('clipboard', '1.0', 'Clipboard DB', 2 * 1024 * 1024);
        
        // Onload
        create_local_table(); // Create table called clips
        get_data(); // Retrieve data from table and display
        
        // Function to create a table in the database
        function create_local_table(){
            db.transaction (
                function(transaction) {
                    transaction.executeSql("CREATE TABLE IF NOT EXISTS clips(text TEXT NOT NULL)");
                }
            );
        }

        // Function to retrieve information from the database and display it
        function get_data(){
            db.transaction(
                function(transaction){
                    transaction.executeSql('SELECT * FROM clips;', [],
                        function dataHandler(transaction, results){
                            // Reference to the HTML element that'll hold the data
                            var container = document.getElementById('container');

                            // Add content to the container with retrieved rows
                            var html = "<h2>Contents</h2>";
                            for(var i = 0; i < results.rows.length; i++){
                                html += results.rows.item(i).text + "<br>";
                            }

                            container.innerHTML = html;
                        }
                    );
                }
            );
        }

        // Function to select a specific paragraph
        function select(para_num){
            // Create a Range object
            var range = document.createRange();

            // Gets the Selection object
            var selection = window.getSelection();

            // Specifies what is to be selected
            range.selectNodeContents(document.getElementById('para' + para_num));

            // Removes any previous selections
            selection.removeAllRanges();

            // Adds the selection to only the given range
            selection.addRange(range);
        }

        function copy_store(para_num){
            // Select text for the specific paragraph
            select(para_num);

            // Get text from <p> tag
            var text = document.getElementById('para' + para_num).innerHTML;

            try {
                // Trigger copy event
                document.execCommand('copy');
            } catch (err) {
                console.log('Error: ' + err);
            }

            // Added copied text to the DB
            db.transaction(
                function(transaction) {
                    transaction.executeSql('INSERT INTO clips (text) VALUES ("' + text + '");');
                },
            );
        }

        // Detect when text is copied
        document.addEventListener('copy', function(e){
            alert('copied');
        });
    </script>
</html>
```

#### Using Local Storage
```html
<!DOCTYPE html>
<html>
    <head>
	<title>Clipboard & Local Storage</title>
    </head>
    <body>
        <!-- Dummy paragraphs to be able to copy some text -->
        <p id="para1">1. This is paragraph 1</p>
        <button id="copy_btn" onclick="copy_store(1)">Copy</button>

        <p id="para2">2. This is paragraph 2</p>
        <button id="copy_btn" onclick="copy_store(2)">Copy</button>

        <p id="para3">3. This is paragraph 3</p>
        <button id="copy_btn" onclick="copy_store(3)">Copy</button>

        <p id="para4">4. This is paragraph 4</p>
        <button id="copy_btn" onclick="copy_store(4)">Copy</button>
        
        <div id="container"></div>
    </body>
    <script>
        // Display all the previously copied text on the screen
        display_clipboard();

        // Function to select a specific paragraph
        function select(para_num){
            // Create a Range object
            var range = document.createRange();

            // Gets the Selection object
            var selection = window.getSelection();

            // Specifies what is to be selected
            range.selectNodeContents(document.getElementById('para' + para_num));

            // Removes any previous selections
            selection.removeAllRanges();

            // Adds the selection to only the given range
            selection.addRange(range);
        }

        // Function to copy selected text and store information
        function copy_store(para_num){
            // Select text for the specific paragraph
            select(para_num);

            var text = document.getElementById('para' + para_num).innerHTML;

            // Store the key-value pair in local storage
            // Args list: setItem(key, value)
            localStorage.setItem('para' + para_num, text);
            
            try {
                // Trigger copy event
                document.execCommand('copy');
            } catch (err) {
                console.log('Error: ' + err);
            }
        }

        // Function to display the clipboard
        function display_clipboard(){
            var html = "<h1>Contents</h1>";

            // Get all stored keys from localStorage
            keys = Object.keys(localStorage);

            // For each item, display it's value
            for(var i = 0; i < localStorage.length; i++){
                html += localStorage.getItem(keys[i]) + "<br>";
            }

            document.getElementById('container').innerHTML = html;
        }
    </script>
</html>
```
## Device Motion (Using p5.js)
The DeviceMotionEvent provides web developers with information about the speed of changes for the device's position and orientation. P5.js can be found [here](https://p5js.org/).

### Demo
The following demo shows how to make a ball move according to the device's motion.
**NOTE**: This demo requires p5.js. Use the given CDN or download the library
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Accelerometer using p5.js</title>
    </head>
    <body>
    </body>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.6.1/p5.js"></script>
    <script>
        var x, y, z;
        var xpos, ypos;
        
        // Function to set up the canvas
        function setup()  {
            // Set canvas size
            createCanvas(400, 400);

            // Default values
            xpos = 200;
            ypos = 200;
            x = 0;
            y = 0;
        }

        function draw() {
            // set background color to white
            background(255);

            // Add/subract xpos and ypos
            xpos = xpos + x;
            ypos = ypos - y;

            // Wrap ellipse if over bounds
            if(xpos > 400) { xpos = 0; }
            if(xpos < 0) { xpos = 400; }
            if(ypos > 400) { ypos = 0; }
            if(ypos < 0) { ypos = 400; }

            // Draw ellipse
            fill(255, 0, 0);
            ellipse(xpos, ypos, 25, 25);

            // Display variables
            fill(0);
            noStroke();
            text("x: " + x, 25, 25);
            text("y: " + y, 25, 50);
            text("z: " + z, 25, 75); 
        }

        // Accelerometer Data
        window.addEventListener('devicemotion', function(e) {
            // Get accelerometer values
            x = parseInt(e.accelerationIncludingGravity.x);
            y = parseInt(e.accelerationIncludingGravity.y);
            z = parseInt(e.accelerationIncludingGravity.z); 
        });
    </script>
</html>
```

For reference, [visit this link.](https://developer.mozilla.org/en-US/docs/Web/API/DeviceMotionEvent)
