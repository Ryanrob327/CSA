---
comments: true
layout: post
title: Tri 1 - Individual Review Ticket
description: 
courses: { calendar: {week: 12} }
type: reflection
---

# Individual code
## Java code

```java
password = passwordEncoder.encode(password);
List<Human> humans = repository.findAll();
for (Human hman : humans){
    usedClassCodes.add(hman.getClassCode());
}
String classCode = "";  
if (role.equals("Teacher")){

    int CODE_LENGTH = 6; 
    SecureRandom random = new SecureRandom();
    BigInteger randomBigInt;
    do {
        randomBigInt = new BigInteger(50, random);
        classCode = randomBigInt.toString(32).toUpperCase().substring(0, CODE_LENGTH);
    } while (usedClassCodes.contains(classCode));
    usedClassCodes.add(classCode);
}
else{
    if (role.equals("Student")){
    classCode = null;}
    else{
        return new ResponseEntity<>("The role has to be either 'Student' or 'Teacher'", HttpStatus.BAD_REQUEST);
    }
}

// A Human object WITHOUT ID will create a new record with default roles as student
Human human = new Human(email, password, name, dob, role);
human.setClassCode(classCode);
repository.save(human);
return new ResponseEntity<>(email +" created successfully", HttpStatus.CREATED);
```
Javascript on the frontend:
```javascript
document.getElementById('signInForm').addEventListener('submit', function(event) {
    event.preventDefault();

    var email = document.getElementById('email').value;
    var password = document.getElementById('password').value;

    // Make a POST request to your authentication endpoint
    fetch('https://cj-backend.stu.nighthawkcodingsociety.com/human/authenticate', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            email: email,
            password: password
        })
    })
    .then(response => response.json())
    .then(data => {
        // Assuming the response contains a 'token' field
        var token = data.token;
        // Store the token in localStorage or a cookie for future use
        // For simplicity, we're using localStorage here
        console.log(token);
        localStorage.setItem('token', token);

        alert('Sign in successful!');
    })
    .catch(error => console.error('Error:', error));
});
```

### sign in feature working:

![Image](https://github.com/aidenhuynh/cj_frontend/assets/20897400/bccc1afe-b06b-46d1-853f-767b5ae3580b)

## Insights

Backend:
![Image]({{site.baseurl}}/images/CJ-backend-insights.png)
Frontend:
![Image]({{site.baseurl}}/images/Cj-frontend-insights.png)
Blog:
![Image]({{site.baseurl}}/images/CSA-insights.png)

# individual blog

## College board exam

![Image]({{site.baseurl}}/images/Tri-1-CSA-MC.png)