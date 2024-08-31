
1. GET Request Tests
Verify successful response for a valid GET request.
Verify status code is 200 for a valid GET request.
Verify response time is within acceptable limits for a GET request.
Verify response contains all expected fields for a valid GET request.
Verify the correct content-type is returned for a GET request.
Verify handling of invalid resource in a GET request (404 status code).
Verify GET request with query parameters returns expected results.
Verify GET request with headers (e.g., Authorization) returns expected results.
Verify GET request with path parameters returns expected results.
Verify GET request handles pagination correctly (if applicable).
2. POST Request Tests
Verify successful creation of a resource with a valid POST request.
Verify status code is 201 for a valid POST request.
Verify response body contains the newly created resource in a POST request.
Verify handling of invalid input data in a POST request (400 status code).
Verify duplicate resource creation is handled correctly (409 status code).
Verify the correct content-type is returned for a POST request.
Verify POST request with headers (e.g., Authorization) returns expected results.
Verify POST request handles large payloads correctly.
Verify response time is within acceptable limits for a POST request.
Verify POST request with missing mandatory fields returns a proper error message.
3. PUT Request Tests
Verify successful update of a resource with a valid PUT request.
Verify status code is 200 for a valid PUT request.
Verify response body contains the updated resource in a PUT request.
Verify handling of updating a non-existent resource (404 status code).
Verify PUT request with partial data updates the resource correctly.
Verify PUT request with invalid input data returns a proper error message (400 status code).
Verify PUT request with missing mandatory fields returns a proper error message.
Verify response time is within acceptable limits for a PUT request.
Verify PUT request with headers (e.g., Authorization) returns expected results.
Verify the correct content-type is returned for a PUT request.
4. DELETE Request Tests
Verify successful deletion of a resource with a valid DELETE request.
Verify status code is 204 for a valid DELETE request.
Verify response body is empty after a DELETE request.
Verify handling of deleting a non-existent resource (404 status code).
Verify DELETE request with headers (e.g., Authorization) returns expected results.
Verify deletion of a resource is permanent (subsequent GET returns 404).
Verify DELETE request returns an appropriate error message when trying to delete a resource linked to other data.
Verify response time is within acceptable limits for a DELETE request.
Verify multiple DELETE requests for the same resource return consistent results.
Verify the correct content-type is returned for a DELETE request.
5. Authentication & Authorization Tests
Verify API returns 401 for unauthorized access without a token.
Verify API returns 403 for access with insufficient permissions.
Verify valid token allows access to a protected resource.
Verify token expiration leads to a 401 response.
Verify API handles invalid tokens correctly (e.g., tampered or expired).
6. Edge Case & Miscellaneous Tests
Verify API handles special characters in request parameters correctly.
Verify API handles large payloads efficiently.
Verify API handles rate-limiting correctly (429 status code).
Verify API behavior with multiple concurrent requests.
Verify API handles unexpected input gracefully (e.g., null, empty strings, negative numbers).


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

4. Verify handling of invalid resource in a GET request (404 status code):

   public void testInvalidResourceGetRequest() {

   given()

        .baseUri("https://api.example.com")

   .when()

        .get("/resource/9999")
   .then()

        .statusCode(404);
}

5. Verify GET request with query parameters returns expected results:

   public void testGetRequestWithQueryParams() {

    given()

        .baseUri("https://api.example.com")

        .queryParam("status", "active")

    .when()

        .get("/resources")
    .then()

        .statusCode(200)

        .body("size()", greaterThan(0));
}

6. Verify successful creation of a resource with a valid POST request:

public void testValidPostRequest() {
   
    String requestBody = "{ \"name\": \"New Resource\", \"status\": \"active\" }";
    
    given()
       
        .baseUri("https://api.example.com")
        
        .contentType("application/json")
        
        .body(requestBody)
    .when()
        .post("/resource")
    .then()
        
        .statusCode(201)
        
        .body("id", notNullValue())
        
        .body("name", equalTo("New Resource"))
        
        .body("status", equalTo("active"));
}

7. Verify handling of invalid input data in a POST request (400 status code):

   public void testInvalidPostRequest() {
    String requestBody = "{ \"name\": \"\", \"status\": \"active\" }";
    
    given()
        .baseUri("https://api.example.com")
        .contentType("application/json")
        .body(requestBody)
    .when()
        .post("/resource")
    .then()
        .statusCode(400);
}


8. Verify duplicate resource creation is handled correctly (409 status code):

   public void testDuplicatePostRequest() {
    String requestBody = "{ \"name\": \"Duplicate Resource\", \"status\": \"active\" }";
    
    given()
        .baseUri("https://api.example.com")
        .contentType("application/json")
        .body(requestBody)
    .when()
        .post("/resource")
    .then()
        .statusCode(201); // First creation
    
    given()
        .baseUri("https://api.example.com")
        .contentType("application/json")
        .body(requestBody)
    .when()
        .post("/resource")
    .then()
        .statusCode(409); // Duplicate creation
}

9. Verify successful update of a resource with a valid PUT request:

    public void testValidPutRequest() {
    String requestBody = "{ \"name\": \"Updated Resource\", \"status\": \"inactive\" }";
    
    given()
        .baseUri("https://api.example.com")
        .contentType("application/json")
        .body(requestBody)
    .when()
        .put("/resource/1")
    .then()
        .statusCode(200)
        .body("id", equalTo(1))
        .body("name", equalTo("Updated Resource"))
        .body("status", equalTo("inactive"));
}

10. Verify handling of updating a non-existent resource (404 status code):

    public void testUpdateNonExistentResource() {
    String requestBody = "{ \"name\": \"Non-existent Resource\", \"status\": \"inactive\" }";
    
    given()
        .baseUri("https://api.example.com")
        .contentType("application/json")
        .body(requestBody)
    .when()
        .put("/resource/9999")
    .then()
        .statusCode(404);
}

11. Verify successful deletion of a resource with a valid DELETE request:

    public void testValidDeleteRequest() {
    given()
        .baseUri("https://api.example.com")
    .when()
        .delete("/resource/1")
    .then()
        .statusCode(204); // No content on successful deletion
}

12. Verify handling of deleting a non-existent resource (404 status code):

    public void testDeleteNonExistentResource() {
    given()
        .baseUri("https://api.example.com")
    .when()
        .delete("/resource/9999")
    .then()
        .statusCode(404);
}

13. Verify API returns 401 for unauthorized access without a token:

    public void testUnauthorizedAccess() {
    given()
        .baseUri("https://api.example.com")
    .when()
        .get("/secure/resource")
    .then()
        .statusCode(401);
}

14. Verify valid token allows access to a protected resource:

public void testAuthorizedAccess() {
    String validToken = "Bearer valid_token_example";
    
    given()
        .baseUri("https://api.example.com")
        .header("Authorization", validToken)
    .when()
        .get("/secure/resource")
    .then()
        .statusCode(200);
}

15. Verify API handles special characters in request parameters correctly:
    
