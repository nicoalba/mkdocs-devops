# Example code

## Example code

```java
    // Called when the submit button is clicked.
    function submitLine() {
    var patientLine = textbox.value;
    therapySession += "<p> <em>" + patientLine + "</em> </p>";

    // Choose response based on last character.
    var therapistLine;
    if (lastChar(patientLine) == "?") {
        therapistLine = randomElement(questionResponses);
    } 
    else if (lastChar(patientLine) == "!") {
        therapistLine = randomElement(exclamationResponses);
    } 
    else {
        therapistLine = createQuestion(patientLine);
    }

    // Still no good response, so use a basic response.
    if (therapistLine == null) {
        therapistLine = randomElement(genericResponses);
    }

    therapySession += "<p>" + therapistLine + "</p>";

    conversation.innerHTML = therapySession;
}
```