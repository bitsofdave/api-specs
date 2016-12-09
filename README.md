# api-specs
Swagger Specs for the Namely API

# Adding or Modifying the Namely API
1. Pull the latest from master
2. Make a PR against master
4. Refer to the [Swagger Specification](http://swagger.io/specification/)

# Deploying to Stoplight.io
1. Login to Stoplight.io
2. Find the drop down on the left nav bar and click on "Select an API"
3. Select "Namely API"
4. Click on "Scenarios & HTTP" on the left nav bar at the very end of the list
5. Underneath the "Scenarios" header, click on the folder titled "Sync Github --> Stoplight"
6. Click on the blue button titled "Run Scenario"
  * If successful, you will receive a 200 status code
  * If unsuccessful, you will receive a 400 status code with a descriptive error message on what to fix
7. Verify that the "Try It Out" feature is enabled, along with the "Security Schema" for an API Key in order to have the interactive sandbox
8. Verify that all endpoints have the API Key feature enabled, if not, go to "Design", and enable it for each endpoint underneath "Authentication"
9. Go to "Hosted Docs" in the left nav bar and preview the documentation before publishing
10. Once verified, stay on "Hosted Docs" and click the "Re-Publish" button
