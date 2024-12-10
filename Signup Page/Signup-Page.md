# Newsletter Sign-Up Page

This is a fully functional newsletter sign-up form using AMPscript and HTML, designed for Salesforce Marketing Cloud.

## AMPscript Code

```html
%%[
Var @firstname, @lastname, @email, @phone, @country, @dataExtension
Set @firstname= RequestParameter("firstname")
Set @lastname= RequestParameter("lastname")
Set @email= RequestParameter("email")
Set @phone= RequestParameter("phone")
Set @country= RequestParameter("country")
Set @dataExtension= "Newsletter_Signup"

IF NOT EMPTY(@firstname) AND NOT EMPTY(@lastname) AND NOT EMPTY(@email) AND NOT EMPTY(@country) THEN
InsertData(@dataExtension, "First_Name", @firstname, "Last_Name", @lastname, "Email_Address", @email, "Phone_Number", @phone, "Country", @country)
Redirect("https://mc86w70sbqmb0kccf2-2xvwcn024.pub.sfmc-content.com/kr2zar0nhhq")
ENDIF
]%%

         
<title>Newsletter Sign-Up</title>
    <style>
  body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
        }
        .container {
            width: 80%;
            max-width: 500px;
            margin: 50px auto;
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        .header {
            text-align: center;
            margin-bottom: 10px;
        }
        .header img {
            width: 180px;
            
        }
        h1 {
            color: #333;
        }
        .form-group {
            margin-bottom: 15px;
          margin-right:21px;
        }
        label {
            font-weight: bold;
        }
        input[type="text"], input[type="email"], input[type="tel"] {
            width: 100%;
            padding: 10px;
          
            font-size: 14px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
      
       .form-group-btn {
            margin-bottom: 15px;
;
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
        }
        .form-button:hover {
            background-color: #0056b3;
        }
      
        }
    </style>
       <div class="container">
        <div class="header">
            <img src="https://image.s7.sfmc-content.com/lib/fe3211717d64047a741073/m/1/SMFC+Logo.jpeg" alt="Logo">
            <h1>Sign Up for Our Newsletter</h1>
            <p>Stay updated with the latest news and offers.</p>
        </div>
        <!-- Start of the Sign-Up Form -->
        <form action="%%=RequestParameter('PAGE_URL')=%%" method="post">
            <!-- First Name -->
            <div class="form-group">
                <label for="firstName">First Name</label>
                <input type="text" id="firstName" name="firstName" required="">
            </div>
            <!-- Last Name -->
            <div class="form-group">
                <label for="lastName">Last Name</label>
                <input type="text" id="lastName" name="lastName" required="">
            </div>
            <!-- Email Address -->
            <div class="form-group">
                <label for="email">Email Address</label>
                <input type="email" id="email" name="email" required="">
            </div>
            <!-- Phone Number -->
            <div class="form-group">
                <label for="phone">Phone Number (Optional)</label>
                <input type="tel" id="phone" name="phone">
            </div>
           <div class="form-group">
             <label for="country">Country</label>
                <input type="text" id="country" name="country" required="">
            </div>
            <!-- Submit Button -->
            <div class="form-group-btn">
                <button type="submit" class="form-button">Subscribe Now</button>
            </div>
        </form>
  </div>
```


### Resources

Visit the [Signup Page](https://mc86w70sbqmb0kccf2-2xvwcn024.pub.sfmc-content.com/cwhvkyu1lh4) for more information.
    
Enjoy building your sign-up form!