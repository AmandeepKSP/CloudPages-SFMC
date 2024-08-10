### Registration Form

This Cloud Page contains an AMPscript form that captures user registration details and inserts them into a Data Extension. After the form submission, users are redirected to a thank you page.

#### Soltuion

```html
%%[
  IF RequestParameter("submitted") == "true" THEN
    SET @firstName = RequestParameter('firstName')
    SET @lastName = RequestParameter('lastName')
    SET @email = RequestParameter('email')
    INSERTDE("Registration Page", "First Name", @firstName, "Last Name", @lastName, "Email", @email)
    Redirect("https://mc86w70sbqmb0kccf2-2xvwcn024.pub.sfmc-content.com/kr2zar0nhhq")
  ENDIF
]%%
        <form method="post">
        <input type="hidden" name="submitted" value="true">
            <label for="firstName">First Name</label><br>
            <input type="text" name="firstName" required=""><br>
            <label for="lastName">Last Name</label><br>
            <input type="text" name="lastName" required=""><br>
            <label for="email">Email</label><br>
            <input type="email" name="email" required=""><br>
            <input type="submit" value="Register">
         </form>
         
```
### Resources

Click on the link for `https://mc86w70sbqmb0kccf2-2xvwcn024.pub.sfmc-content.com/hosdoo0hou3` Cloud pagge.
    
    
  

