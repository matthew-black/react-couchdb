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

### Fetching All the Bear Document Metadata w/ HTTP GET:
* This is kinda like a basic `SELECT * FROM bears`, except it just gives us metadata about each bear document in our `bears` database:
    * ```zsh
      curl -XGET http://admin:prime@localhost:5984/bears/_all_docs
      ```

### Fetching All the Bear Documents w/ HTTP GET:
* This is the `SELECT * FROM bears` replacement:
* Note: Need to wrap the URL in quotes if it includes query params.
    * ```zsh
      curl -XGET 'http://admin:prime@localhost:5984/bears/_all_docs?include_docs=true'
      ```

### Fetching One Bear Document w/ HTTP GET:
* This is like a basic `SELECT * FROM bears WHERE id=someId`:
    * ```zsh
      curl -XGET http://admin:prime@localhost:5984/bears/16c0fe95d43829e068750a054c007aca
      ```
    * 👆 `/bears/[_id value of a bear document goes herer]

### Creating a Bear Document w/ HTTP POST:
* Here's how we can add Winnie the Pooh to our `bears` database via the command line. (`cURL` is just a tool that lets us make HTTP requests from the command line. It's like the most minimal possible version of Postman.)
    * ```zsh
      curl -XPOST http://admin:prime@localhost:5984/bears/ -H "Content-Type: application/json" -d '{"name": "Winnie the Pooh","color": "Yellow","isRealBear": false}'
      ```

### Updating a Bear Document w/ HTTP PUT:
* **Important**: When doing an update, you can't just update a single property (like we can update a single column's value w/ SQL). When updating a document, you have to provide the entire desired "new revision" of that document.
* To update a document, you must provide both the `"_id"` and `"_rev"` values for the document you're targeting.
* Here's how we could bring Winnie the Pooh to life (by flipping `"isRealBear"` to `true`):
      * ```zsh
      curl -XPUT http://admin:prime@localhost:5984/bears/da39956cac2be9f8b907ccdc2b004560 -H "Content-Type: application/json" -d '{"_id": "da39956cac2be9f8b907ccdc2b004560","_rev": "1-89e5a855255b63f1b63afeb0cde8c4e9","name": "Winnie the Pooh","color": "Yellow","isRealBear": true}'
      ```

### Deleting a Bear Document w/ HTTP DELETE:
* To delete a document, you must provide both the `"_id"` (in the URL) and `"_rev"` (in a query parameter) values for the document you're targeting.
* It turns out that bringing Winnie the Pooh to life was a mistake. All the honey is gone. Here's how we delete Winnie the Pooh:
      * ```zsh
      curl -XDELETE 'http://admin:prime@localhost:5984/bears/da39956cac2be9f8b907ccdc2b004560?rev=2-5810eab404de7811d4778943ffbeabd3'
      ```
      
---
