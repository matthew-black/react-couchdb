# CouchDB CRUD

---

## Usage:

##### Get the React App Up and Running:
1. Fork and clone this repo.
2. `cd` into the repo.
3. `npm install` to install dependencies.
4. `npm run dev` to start the Vite server that runs the React app.

##### Install CouchDB and Create a Bears Database:
1. Download and install [CouchDB](https://couchdb.apache.org/#download) by dragging it into your applications folder.
2. Open CouchDB from your applications folder.
3. Create an account when prompted.
4. Log-in with your new account.
5. Create a database named `bears`.
6. Click your newly created `bears` database.
7. Click the green Create Document button.
    * This will create a JSON object that has a system-generated `"id"` value.
    * Copy and paste the `name`, `color`, and `isRealBear` properties into this new JSON document to make a bear. Should look like this:
    *   ```JSON
        {
          "_id": "16c0fe95d43829e068750a054c007aca",
          "name": "Wish Bear",
          "color": "Blue",
          "isRealBear": false
        }
        ```
    * Note: You'll need to add a comma to the end of the `"_id"` line.
    * 
8. Make another bear document:
    *   ```JSON
        {
          "_id": "16c0fe95d43829e068750a054c008852",
          "name": "Kodiak Bear",
          "color": "Brown",
          "isRealBear": true
        }
        ```
9. Woo. That's enough.

---

## Some Context:

### What's a Vite?
* Yeah, it seems nifty. I followed [these docs](https://vitejs.dev/guide/).
* This React app was generated with `npm create vite@latest`.
  * ![](./readme_assets/react_app_via_vite.gif)
  * I also deleted some unnecessary CSS and stuff. 🙂

### What's a Couch?
* It's a NoSQL database, obviously.
* Instead of tables that have columns and rows, CouchDB stores data as JSON documents inside a database.
* ![](./readme_assets/couch_api1.jpeg)
* ![](./readme_assets/couch_api2.jpeg)
* It provides pretty unique functionality. Rather than SQL transactions (ACID, remember?), CouchDB leverages the idea of *revisions*.
* Please consider this video to be required viewing before playing around with CouchDB:
  * https://youtu.be/h9ZSEpv3d9g
  * It's presented by a person who built a pg-style library that makes it easy for a Go app to interact with CouchDB, and he also maintains the CouchDB documentation.
  * Those two 👆 screenshots are from this video.

---

## How We Use HTTP to Interact w/ CouchDB:

### Fetching All the Bear Documents w/ HTTP GET:
* This is kinda like a basic `SELECT * FROM bears`, except it just gives us metadata about each bear document in our `bears` database:
    * ```zsh
      curl -XGET http://admin:prime@localhost:5984/bears/_all_docs
      ```

### Creating a Bear Document w/ HTTP POST:
* Here's how we can add Winnie the Pooh to our `bears` database via the command line. (`cURL` is just a tool that lets us make HTTP requests from the command line. It's like the most minimal possible version of Postman.)
    * ```zsh
      curl -XPOST http://admin:prime@localhost:5984/bears/ -H "Content-Type: application/json" -d '{"name": "Winnie the Pooh","color": "Yellow","isRealBear": false}'
      ```
  
---
