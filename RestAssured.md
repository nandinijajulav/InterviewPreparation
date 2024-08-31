1. GET Request Tests

1. Verify successful response for a valid GET request:

import static io.restassured.RestAssured.*;

import static org.hamcrest.Matchers.*;

public void testValidGetRequest() {

    given()
      .baseUri("https://api.example.com")
    .when()
      .get("/resource/1")
    .then()
      .statusCode(200);
}

2. Verify status code is 200 for a valid GET request:

public void testStatusCodeForGetRequest() {
    
    given()
    
        .baseUri("https://api.example.com")
    
    .when()
    
        .get("/resource/1")
    
    .then()
    
        .assertThat()
        
        .statusCode(200);
}

3. Verify response contains all expected fields for a valid GET request:

   public void testResponseContainsExpectedFields() {

    given()

        .baseUri("https://api.example.com")

    .when()

        .get("/resource/1")

    .then()

        .assertThat()

        .body("id", equalTo(1))

        .body("name", notNullValue())

        .body("status", equalTo("active"));
}

5. Verify handling of invalid resource in a GET request (404 status code):

   public void testInvalidResourceGetRequest() {

    given()

        .baseUri("https://api.example.com")

    .when()

        .get("/resource/9999")

    .then()

        .statusCode(404);
}
