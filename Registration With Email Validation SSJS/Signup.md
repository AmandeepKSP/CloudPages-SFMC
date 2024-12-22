# Newsletter Sign-Up Form with AMPscript, JavaScript, and REST API Integration

This page contains the code for a Newsletter Sign-Up form that integrates AMPscript for data handling and JavaScript for interacting with the Salesforce Marketing Cloud API. The form validates the email address through the REST API and stores the user information into a data extension if the email is valid.

### Code Snippet
```html
%%[
Var @firstname, @lastname, @email, @phone, @country, @submitted

Set @firstname = RequestParameter("firstName")
Set @lastname = RequestParameter("lastName")
Set @email = RequestParameter("email")
Set @phone = RequestParameter("phone")
Set @country = RequestParameter("country")
Set @submitted = RequestParameter("submitted")
]%%

<script runat="server" language="javascript">
Platform.Load("Core", "1");

if (Variable.GetValue("@submitted") === "true") {
    var authURL_ENDPOINT = "XXX/v2/token";
    var payload = '{"grant_type": "client_credentials","client_id": "XXX","client_secret": "XXX"}';
    
    var requestResponse = HTTP.Post(authURL_ENDPOINT, "application/json", payload);
    
    if (requestResponse.StatusCode == 200) {
        var requestResponseJSON = Platform.Function.ParseJSON(requestResponse.Response[0]);
        var access_token = requestResponseJSON.access_token;
        var rest_Url = requestResponseJSON.rest_instance_url;
        var emailValidateURL = rest_Url + "address/v1/validateEmail";
        
        var emailValue = Variable.GetValue("@email");
        var firstNameValue = Variable.GetValue("@firstname");
        var lastNameValue = Variable.GetValue("@lastname");
        var phoneValue = Variable.GetValue("@phone");
        var countryValue = Variable.GetValue("@country");
        
        var emailPayload = '{"email": "' + emailValue + '","validators":["SyntaxValidator","MXValidator","ListDetectiveValidator"]}';
        var emailHeaderName = ["Authorization"];
        var emailHeaderValue = ["Bearer " + access_token];
        
        var requestResponseEV = HTTP.Post(emailValidateURL, "application/json", emailPayload, emailHeaderName, emailHeaderValue);
        
        if (requestResponseEV.StatusCode == 200) {
            var requestResponseEV_Json = Platform.Function.ParseJSON(requestResponseEV.Response[0]);
            if (requestResponseEV_Json.valid) {
                Platform.Function.InsertData("Newsletter_Signup", ["First_Name", "Last_Name", "Email_Address", "Phone_Number", "Country"],
                [firstNameValue, lastNameValue, emailValue, phoneValue, countryValue]);
            } else {
                Redirect("https://mc86w70sbqmb0kccf2-2xvwcn024.pub.sfmc-content.com/njrbl2nncgd", false);
            }
        } else {
            Write("<p>Error validating email. Status code: " + requestResponseEV.StatusCode + "</p>");
        }
    } else {
        Write("<p>Error fetching access token. Status code: " + requestResponse.StatusCode + "</p>");
    }
}
</script>

<title>Newsletter Sign-Up</title>
<style>
    body {
        font-family: sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
    }
    .container {
        width: 80%;
        max-width: 500px;
        background-color: white;
        padding: 30px;
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
    .header {
        text-align: center;
        margin-bottom: 20px;
    }
    .header img {
        width: 180px;
        margin-bottom: 10px;
    }
    h1 {
        color: #333;
        margin: 10px 0;
    }
    .form-group {
        margin-bottom: 15px;
    }
    label {
        font-weight: bold;
        display: block;
        margin-bottom: 5px;
    }
    input[type="text"], input[type="email"], input[type="tel"] {
        width: 100%;
        padding: 10px;
        font-size: 14px;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
    }
    .form-group-btn {
        margin-top: 20px;
    }
    .form-button {
        width: 100%;
        padding: 12px;
        background-color: #009fdf;
        color: white;
        border: none;
        font-size: 16px;
        cursor: pointer;
        border-radius: 4px;
        transition: background-color 0.3s;
    }
    .form-button:hover {
        background-color: #0056b3;
    }
</style>

<div class="container">
    <div class="header">
        <img src="https://image.s7.sfmc-content.com/lib/fe3211717d64047a741073/m/1/SMFC+Logo.jpeg" alt="Logo">
        <h1>Sign Up for Our Newsletter</h1>
        <p>Stay updated with the latest news and offers.</p>
    </div>
    <form action="%%=RequestParameter('PAGE_URL')=%%" method="post">
        <div class="form-group">
            <label for="firstName">First Name</label>
            <input type="text" id="firstName" name="firstName" required="">
        </div>
        <div class="form-group">
            <label for="lastName">Last Name</label>
            <input type="text" id="lastName" name="lastName" required="">
        </div>
        <div class="form-group">
            <label for="email">Email Address</label>
            <input type="email" id="email" name="email" required="">
        </div>
        <div class="form-group">
            <label for="phone">Phone Number (Optional)</label>
            <input type="tel" id="phone" name="phone">
        </div>
        <div class="form-group">
            <label for="country">Country</label>
            <input type="text" id="country" name="country" required="">
        </div>
        <div class="form-group-btn">
            <input type="hidden" name="submitted" value="true">
            <button type="submit" class="form-button">Subscribe Now</button>
        </div>
    </form>
</div>
```
### Replace the placeholder values:

* XXXX/v2/token: Replace XXXX with your authentication URL.
* XXX: Replace with your Client ID and Client Secret.