# vidijs-postman

A list of postman collections generated from the WADL available from the Vidispine API server.

## Importing Collection

* These collections can be imported into Postman either by downloading the JSON files
or use the raw URL from Github.
* Each version fo the API server will have a unique collection with the suffix `.postman_collection.json`.

## Managing Environments

Collections use environment variables to store the url and authentication details.
An example environment can be imported from `localhost.postman_environment.json`

* `VIDISPINE_URL` - The URL to the API server, including the scheme (`http` or `https`) and port.
  - `http://localhost:8080`
  - `https://demo.myvidispine.com`
* `VIDISPINE_USER` - The user to use with basic authentication
  - `admin`
* `VIDISPINE_PASSWORD` = The password of the user
  - `admin`

## Generating Collection

This example uses the API server hosted at `http://localhost:8080`
with the credentials `admin:admin` and the version `4.17.0`

* Create new version folder
```
mkdir 4.17.0
cd 4.17.0
```

* Download the WADL from the root, API and APInoauth endpoints
```
curl -o root.wadl 'http://localhost:8080/application.wadl'
curl -o APInoauth.wadl 'http://localhost:8080/APInoauth/application.wadl'
curl -o API.wadl -u admin:admin 'http://localhost:8080/API/application.wadl'
```

* Open Postman, click the 'Import' button, choose 'Import File' tab.
* Import `root.wadl`.  This may take a moment to import.
* A new Postman collection called `Converted From WADL` will be created.
* Rename the new collection to match the API version, eg `Vidispine - 4.17.0`.
* Edit the collection.
 - Open the `Authorization` tab. Set `Username` to `{{VIDISPINE_USER}}` and `Password` to `{{VIDISPINE_PASSWORD}}`
 - Open the `Variable` tab.  Set the variable `VIDISPINE_USER` with the value `admin` and `VIDISPINE_PASSWORD` to the value `admin`.
 - Click the `Update` button to save.
* Delete the `APInoauth` and `API` folders and create new empty folders with the same names
* Import `APInoauth.wadl`.  This will create a new collection called `Converted From WADL`.
* Drag the subfolders from `Converted From WADL` into the `APInoauth` folder in the root collection.
* Repeat these steps for the `API.wadl`.
* Export the root collection, choosing `Collection v2.1`.  This will create the file `Vidispine - 4.17.0.postman_collection.json`.
* Rename or delete the root collection.
* Edit the file `Vidispine - 4.17.0.postman_collection.json`
  - Replace `"raw": "http://localhost:8080` with `"raw": "{{VIDISPINE_URL}}`
  - Replace `"name": "http://localhost:8080/` with `"name": "/`
  - Replace (regex) `"host\"\: \[(.*?|\n)*?\]` with `"host": ["{{VIDISPINE_URL}}"]`
  - Replace `"header": []` with `"header": [{"key": "Accept", "value": "application/json"}, {"key": "Content-Type", "value": "application/json"}]`
  - Remove `"protocol": "https",`
  - Remove `"port": "8080",`
* Import the JSON file.  This will create a collection with the originally exported name.
* Export the collection as JSON to this repository to ensure the JSON is reformatted by Postman.
