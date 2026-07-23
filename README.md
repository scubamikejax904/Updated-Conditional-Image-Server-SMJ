# Modified Conditional Image Server for Hubitat by ScubaMikeJax904

Code originally from (taken with mimimal modifications)
Conditional Image Server by Justin Leonard 
Copyright\u00A9 2020 Justin Leonard
 * Many thanks to @dman2306 help in rendering jpg output

This Hubitat app allows you to dynamically serve images (JPG, PNG, SVG, GIF) to your dashboard or external services. It works by reading an image URL from a Rule Machine Global Variable, fetching the image, and serving it via a local API endpoint.

It uses a **Parent/Child app architecture**. The "Manager" app acts as a clean folder to keep your UI organized, while the "Child" apps handle the actual image serving and OAuth tokens independently.

## Prerequisites
* **Rule Machine Global Variable Connector**: You must have this driver installed and a device created to hold your dynamic image URLs.

---

## Installation Instructions

### Step 1: Add the App Code
1. In your Hubitat UI, go to **Apps Code** (under the Developer or Apps menu).
2. Click **New App**.
3. Name it `Conditional Image Server Manager`, set the Namespace to your own (e.g., `your_namespace`), and paste the **Manager App** code. Click **Save**.
4. Click **New App** again.
5. Name it `Conditional Image Server`, use the same Namespace, and paste the **Child App** code. Click **Save**.

### Step 2: Enable OAuth (CRITICAL)
*Note: OAuth is ONLY required for the Child app, not the Manager app.*
1. In the **Apps Code** list, find **Conditional Image Server** (the Child app).
2. Click the **three vertical dots (⋮)** on the far right of its row.
3. Select **OAuth** from the dropdown menu.
4. Click **Enable OAuth** in the dialog that appears.

### Step 3: Install the Manager App
1. Go to **Apps** -> **Add User App** (bottom left).
2. Select **Conditional Image Server Manager** and click **Add**.
3. Click **Done** to open the app's settings.

---

## Setup and Usage

### Creating your Global Variable
1. On Hubitat open setting
2. Open Hub Variables (under Hub Global Values)
3. Click create new Variable (Green plus)
4. Name the Variable ex: Holiday Image
5. For Variable Type use String
6. Set initial value to hello (this is just a placeholder as it needs something set to create the variable)
7. Mouse click anywhere in the window (this finishes creating the variable)
8. Scroll to your newly created variable and click on Create (this creates the connector allowing the Variable to be used in other places in Hubitat)

### Adding an Image Server Instance
1. Open the **Conditional Image Server Manager** app from your User Apps list.
2. Scroll down to the **Image Server Instances** section.
3. Click the **Create Conditional Image** button.
4. **Assign a Name**: Give this specific instance a custom name (e.g., "Front Door Camera Snapshot") so you can easily identify it.
5. **Select Device**: Choose your **Rule Machine Global Variable Connector** device.
6. Click **Done**.

### Getting Your API URL
1. Once the child instance is saved, open it back up from inside the Manager app.
2. You will see a **Local** URL generated (e.g., `http://192.168.x.x/apps/api/123/conditionalImage?access_token=abc...`).
3. Copy this URL and use it in your dashboard (e.g., SharpTools, Hubitat Dashboard) wherever you need the dynamic image to appear.
4. In Hubitat dashboard (original not Easy, I do not use easy dashboards and am unsure of their capabilitis)
   A. Create a tile for an image by clicking the "+"
   B. leave the Pick a Device empty (don't select anything) Pick the Image Template
   C.  Use the above URL as either the background Image Link (this will stretch the image to fill the Tile), or as the Image URL (this            will cause the image to expand or shrink to show the entire image
  **I am unshure if this will work for the Dashboard Background, but I suspect it would as you can use a URL**

### How to Use with Rule Machine
1. In Rule Machine, create a rule that triggers based on your desired conditions.
2. As the action, set the **Global Variable** (via the Global Variable Connector device) to the direct URL of the image you want to display.
   From my experience you must us an Https address, you can setup a github website and server your images from there or use any online image, I do recomend downloading the images as free online links frequently change or are removed, you can use a local NAS to serve images as long as it's accessable through Https.  (I use a Synology NAS with a DDNS address that is only locally accesable that has a certificate for Https access, it's overkill but it works)
3. When your dashboard requests the Local API URL from Step 2, this app will instantly fetch the image from your variable's URL and serve it!

### Adding Multiple Instances
To serve different dynamic images for different purposes, simply go back to the **Conditional Image Server Manager** and click **Create Conditional Image** again. Each instance will generate its own unique API URL and operate completely independently.
