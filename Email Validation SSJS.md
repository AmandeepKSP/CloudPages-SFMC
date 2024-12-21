## Email Validation SSJS

This script demonstrates how to validate an email address using SSJS (Server-Side JavaScript) in Salesforce Marketing Cloud. It collects form data, processes an OAuth token request, and validates the email address by making an API request to an email validation service.

### Script


```Javascript
%%[
SET @first_name = RequestParameter("firstname")
SET @last_name = RequestParameter("lastname")
SET @email = RequestParameter("email")
SET @submitted = RequestParameter("submitted")
]%%

<script runat="server">
// Load the Core library for server-side JavaScript
Platform.Load("Core", "1");

// Start debug output
Write("<h3>Debug Output:</h3>");

// Check if the form has been submitted
if (Variable.GetValue("@submitted") === "true") {
    Write("<p>Form submitted. Processing...</p>");

    // Set up the token endpoint URL
    var url = "XXXX/v2/token";
    // Prepare the payload for token request
    var payload = '{"grant_type": "client_credentials","client_id": "XXX","client_secret": "XXX"}';
    // Make a POST request to get the access token
    var responseResult = HTTP.Post(url, "application/json", payload);
    
    // Print the status code of the token request
    Write("<p>Token request status code: " + responseResult.StatusCode + "</p>");

    // Check if the token request was successful
    if (responseResult.StatusCode == 200) {
        // Parse the JSON response
        var responseJson = Platform.Function.ParseJSON(responseResult.Response[0]);
        // Extract the access token and REST URL
        var accessToken = responseJson.access_token;
        var rest_Url = responseJson.rest_instance_url;

        // Print the first 10 characters of the access token and the full REST URL
        Write("<p>Access Token obtained: " + accessToken.substring(0, 10) + "...</p>");
        Write("<p>REST URL: " + rest_Url + "</p>");

        // Retrieve form data from AMPscript variables
        var firstName = Variable.GetValue("@first_name");
        var lastName = Variable.GetValue("@last_name");
        var email = Variable.GetValue("@email");

        // Print the form data
        Write("<p>Form data:</p>");
        Write("<ul>");
        Write("<li>First Name: " + firstName + "</li>");
        Write("<li>Last Name: " + lastName + "</li>");
        Write("<li>Email: " + email + "</li>");
        Write("</ul>");

        // Set up the email validation API endpoint
        var emailValidateUrl = rest_Url + "/address/v1/validateEmail";
        // Prepare the payload for email validation
        var emailValidatePayload = '{"email":"' + email + '","validators":["SyntaxValidator","MXValidator","ListDetectiveValidator"]}';
        // Set up the headers for the API request
        var emailHeaderName = ["Authorization"];
        var emailHeaderValue = ["Bearer " + accessToken];
        // Make a POST request to validate the email
        var emailValidateResponse = HTTP.Post(emailValidateUrl, "application/json", emailValidatePayload, emailHeaderName, emailHeaderValue);

        // Print the status code of the email validation request
        Write("<p>Email validation request status code: " + emailValidateResponse.StatusCode + "</p>");

        // Check if the email validation request was successful
        if (emailValidateResponse.StatusCode == 200) {
            // Parse the JSON response from email validation
            var emailValidateResponseJson = Platform.Function.ParseJSON(emailValidateResponse.Response[0]);
            // Print the full email validation response
            Write("<p>Email validation response: " + Stringify(emailValidateResponseJson) + "</p>");
            
            // Check if the email is valid
            if (emailValidateResponseJson.valid) {
                Write("<p>Email is valid.</p>");
            } else {
                Write("<p>Email is not valid.</p>");
            }
        } else {
            // Print an error message if email validation failed
            Write("<p>Error validating email. Status code: " + emailValidateResponse.StatusCode + "</p>");
        }
    } else {
        // Print an error message if token request failed
        Write("<p>Error fetching access token. Status code: " + responseResult.StatusCode + "</p>");
    }

    // Indicate that processing is complete
    Write("<p>Processing completed.</p>");
} else {
    // Indicate that the form has not been submitted yet
    Write("<p>Form not submitted yet.</p>");
}
</script>

<h2>Register</h2>
<form action="%%=RequestParameter('PAGEURL')=%%" method="post">
    <label>First Name</label>
    <input type="text" name="firstname" required="">

    <label>Last Name</label>
    <input type="text" name="lastname" required="">

    <label>Email</label>
    <input type="email" name="email" required="">

    <input type="hidden" name="submitted" value="true">
    <input type="submit" value="Submit">
</form>
```
###
Replace the placeholder values:

* XXXX/v2/token: Replace XXXX with your authentication URL.
* XXX: Replace with your Client ID and Client Secret.