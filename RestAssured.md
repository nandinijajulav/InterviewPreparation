
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


9. Verify duplicate resource creation is handled correctly (409 status code):

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

11. Verify successful update of a resource with a valid PUT request:

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

13. Verify handling of updating a non-existent resource (404 status code):

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

15. Verify successful deletion of a resource with a valid DELETE request:

    public void testValidDeleteRequest() {

    given()

        .baseUri("https://api.example.com")

    .when()

        .delete("/resource/1")

    .then()

        .statusCode(204); // No content on successful deletion
}

17. Verify handling of deleting a non-existent resource (404 status code):

    public void testDeleteNonExistentResource() {

    given()

        .baseUri("https://api.example.com")

    .when()

        .delete("/resource/9999")

    .then()

        .statusCode(404);
}

19. Verify API returns 401 for unauthorized access without a token:

    public void testUnauthorizedAccess() {

    given()

        .baseUri("https://api.example.com")

    .when()

        .get("/secure/resource")

    .then()

        .statusCode(401);
}

21. Verify valid token allows access to a protected resource:

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

16. Test Scenario:
Happy Path:

Send a GET request with a valid Authorization header.
Verify that the API returns the expected data with a 200 OK status code.
Invalid Token:

Send a GET request with an invalid or expired token in the Authorization header.
Verify that the API returns a 401 Unauthorized or 403 Forbidden status code.
Missing Token:

Send a GET request without the Authorization header.
Verify that the API returns a 401 Unauthorized status code.

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import io.restassured.response.Response;
import org.junit.jupiter.api.Test;

public class AuthorizationTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String ENDPOINT = "/user/profile";

    // Valid Authorization Token
    private static final String VALID_TOKEN = "Bearer valid_token_here";
    
    // Invalid Authorization Token
    private static final String INVALID_TOKEN = "Bearer invalid_token_here";

    @Test
    public void testGetRequestWithValidAuthorizationHeader() {
        // Send GET request with valid Authorization header
        given()
            .baseUri(BASE_URL)
            .header("Authorization", VALID_TOKEN)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(200)  // Verify the status code is 200 OK
            .body("username", equalTo("johndoe"))  // Example: verify the username in the response
            .body("email", equalTo("johndoe@example.com"))  // Example: verify the email in the response
            .body("roles", hasItem("admin"));  // Example: verify the user has an "admin" role
    }

    @Test
    public void testGetRequestWithInvalidAuthorizationHeader() {
        // Send GET request with invalid Authorization header
        given()
            .baseUri(BASE_URL)
            .header("Authorization", INVALID_TOKEN)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(401)  // Verify the status code is 401 Unauthorized
            .body("error", equalTo("Invalid token"));  // Example: verify the error message
    }

    @Test
    public void testGetRequestWithoutAuthorizationHeader() {
        // Send GET request without Authorization header
        given()
            .baseUri(BASE_URL)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(401)  // Verify the status code is 401 Unauthorized
            .body("error", equalTo("Authorization header missing"));  // Example: verify the error message
    }
}

Verify GET request with path parameters returns expected results.

Purpose of the Test:
Path Parameter Handling: Verify that the API correctly interprets the path parameters and retrieves the correct resource.
Data Validation: Ensure that the response data matches the expected results based on the provided path parameters.
Error Handling: Check that the API returns the correct error status codes (like 404 Not Found) when invalid or non-existent path parameters are used.

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class PathParameterTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String ENDPOINT = "/users/{userId}";

    @Test
    public void testGetRequestWithValidPathParameter() {
        // Send GET request with a valid path parameter (e.g., userId = 123)
        given()
            .baseUri(BASE_URL)
            .pathParam("userId", 123)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(200)  // Verify the status code is 200 OK
            .body("id", equalTo(123))  // Example: verify the user ID in the response
            .body("username", equalTo("johndoe"))  // Example: verify the username in the response
            .body("email", equalTo("johndoe@example.com"));  // Example: verify the email in the response
    }

    @Test
    public void testGetRequestWithInvalidPathParameter() {
        // Send GET request with an invalid path parameter (e.g., userId = 9999, assuming this ID doesn't exist)
        given()
            .baseUri(BASE_URL)
            .pathParam("userId", 9999)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(404)  // Verify the status code is 404 Not Found
            .body("error", equalTo("User not found"));  // Example: verify the error message
    }
}

Purpose of the Test:
Pagination Handling: Verify that the API correctly processes pagination parameters and returns the expected subset of data.
Boundary Testing: Check the behavior of the API when navigating to the first page, last page, or beyond the available data.
Consistency: Ensure that the data returned on consecutive pages is consistent and does not overlap or skip records.

est Scenario:
Basic Pagination Test:

Send a GET request with pagination parameters (e.g., page, limit) and verify that the API returns the correct subset of data.
Boundary Tests:

Test the first and last pages to ensure they are handled correctly.
Test with a page number that exceeds the total number of available pages.
Edge Cases:

Test with a limit of 0 or a negative value.
Test with an invalid page number (e.g., negative or non-integer values).


import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class PaginationTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String ENDPOINT = "/items";

    @Test
    public void testGetRequestWithPaginationParameters() {
        // Send GET request with pagination parameters (e.g., page=1, limit=10)
        given()
            .baseUri(BASE_URL)
            .queryParam("page", 1)
            .queryParam("limit", 10)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(200)  // Verify the status code is 200 OK
            .body("items.size()", equalTo(10))  // Verify that 10 items are returned
            .body("page", equalTo(1))  // Verify the correct page number is returned
            .body("totalItems", greaterThan(10))  // Verify that totalItems is greater than the limit
            .body("totalPages", greaterThan(1));  // Verify that there are multiple pages
    }

    @Test
    public void testGetRequestWithBoundaryPagination() {
        // Test the last page to ensure it returns the correct data
        int lastPage = 5;  // Assume there are 5 pages
        given()
            .baseUri(BASE_URL)
            .queryParam("page", lastPage)
            .queryParam("limit", 10)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(200)
            .body("page", equalTo(lastPage))
            .body("items.size()", lessThanOrEqualTo(10));  // Verify the last page has up to 10 items

        // Test a page number beyond the last page
        given()
            .baseUri(BASE_URL)
            .queryParam("page", lastPage + 1)
            .queryParam("limit", 10)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(200)
            .body("items.size()", equalTo(0));  // Verify that no items are returned
    }

    @Test
    public void testGetRequestWithInvalidPaginationParameters() {
        // Test with a limit of 0
        given()
            .baseUri(BASE_URL)
            .queryParam("page", 1)
            .queryParam("limit", 0)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(400)  // Verify the status code is 400 Bad Request or handled error
            .body("error", containsString("Invalid limit value"));

        // Test with a negative page number
        given()
            .baseUri(BASE_URL)
            .queryParam("page", -1)
            .queryParam("limit", 10)
        .when()
            .get(ENDPOINT)
        .then()
            .statusCode(400)  // Verify the status code is 400 Bad Request or handled error
            .body("error", containsString("Invalid page value"));
    }
}


Verify response time is within acceptable limits for a POST request.

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class ResponseTimeTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String ENDPOINT = "/resources";

    @Test
    public void testPostRequestResponseTime() {
        // JSON body for resource creation
        String requestBody = "{ \"name\": \"TestResource\", \"type\": \"ExampleType\", \"value\": \"12345\" }";

        // Set acceptable response time limit in milliseconds
        long acceptableResponseTime = 2000; // 2 seconds

        // Send POST request and verify response time is within acceptable limits
        given()
            .baseUri(BASE_URL)
            .header("Content-Type", "application/json")
            .body(requestBody)
        .when()
            .post(ENDPOINT)
        .then()
            .statusCode(201)  // Verify the status code is 201 Created
            .time(lessThan(acceptableResponseTime));  // Verify the response time is less than 2000 ms
    }
}

Verify POST request with missing mandatory fields returns a proper error message 

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class MissingMandatoryFieldsTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String ENDPOINT = "/resources";

    @Test
    public void testPostRequestWithMissingMandatoryFields() {
        // JSON body with missing mandatory fields
        String requestBody = "{ \"name\": \"TestResource\" }"; // Assuming "type" and "value" are mandatory

        // Send POST request with missing fields and verify the response
        given()
            .baseUri(BASE_URL)
            .header("Content-Type", "application/json")
            .body(requestBody)
        .when()
            .post(ENDPOINT)
        .then()
            .statusCode(400)  // Verify the status code is 400 Bad Request
            .body("error", equalTo("Missing required fields"))  // Verify the error message
            .body("details", containsString("type"))  // Optionally, check if the error details mention the missing "type" field
            .body("details", containsString("value"));  // Optionally, check if the error details mention the missing "value" field
    }
}

Verify handling of deleting a non-existent resource (404 status code).

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class DeleteNonExistentResourceTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String ENDPOINT = "/resources";

    @Test
    public void testDeleteNonExistentResource() {
        // Resource ID for a non-existent resource
        String nonExistentResourceId = "12345"; // Assume this ID does not exist

        // Send DELETE request for a non-existent resource and verify the response
        given()
            .baseUri(BASE_URL)
        .when()
            .delete(ENDPOINT + "/" + nonExistentResourceId)
        .then()
            .statusCode(404)  // Verify the status code is 404 Not Found
            .body("error", equalTo("Resource not found"))  // Verify the error message
            .body("details", containsString(nonExistentResourceId));  // Optionally, check if the error details mention the resource ID
    }
}

Verify API returns 401 for unauthorized access without a token

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class UnauthorizedAccessTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String PROTECTED_ENDPOINT = "/protected-resource";

    @Test
    public void testUnauthorizedAccessWithoutToken() {
        // Send request without authorization token and verify the response
        given()
            .baseUri(BASE_URL)
        .when()
            .get(PROTECTED_ENDPOINT)
        .then()
            .statusCode(401)  // Verify the status code is 401 Unauthorized
            .body("error", equalTo("Unauthorized"));  // Verify the error message
    }
}

Verify API returns 403 for access with insufficient permissions

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class ForbiddenAccessTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String PROTECTED_ENDPOINT = "/protected-resource";

    @Test
    public void testAccessWithInsufficientPermissions() {
        // Use a token with insufficient permissions
        String insufficientToken = "INVALID_TOKEN_WITHOUT_REQUIRED_PERMISSIONS";

        given()
            .baseUri(BASE_URL)
            .header("Authorization", "Bearer " + insufficientToken)
        .when()
            .get(PROTECTED_ENDPOINT)
        .then()
            .statusCode(403)  // Verify the status code is 403 Forbidden
            .body("error", equalTo("Forbidden"));  // Verify the error message
    }
}


Verify valid token allows access to a protected resource

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class ValidTokenAccessTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String PROTECTED_ENDPOINT = "/protected-resource";

    @Test
    public void testAccessWithValidToken() {
        // Use a valid token
        String validToken = "VALID_TOKEN";

        given()
            .baseUri(BASE_URL)
            .header("Authorization", "Bearer " + validToken)
        .when()
            .get(PROTECTED_ENDPOINT)
        .then()
            .statusCode(200)  // Verify the status code is 200 OK
            .body("message", equalTo("Access granted"));  // Verify the response body
    }
}

Verify token expiration leads to a 401 response

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class TokenExpirationTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String PROTECTED_ENDPOINT = "/protected-resource";

    @Test
    public void testExpiredToken() {
        // Use an expired token
        String expiredToken = "EXPIRED_TOKEN";

        given()
            .baseUri(BASE_URL)
            .header("Authorization", "Bearer " + expiredToken)
        .when()
            .get(PROTECTED_ENDPOINT)
        .then()
            .statusCode(401)  // Verify the status code is 401 Unauthorized
            .body("error", equalTo("Token expired"));  // Verify the error message
    }
}

Verify API handles invalid tokens correctly (e.g., tampered or expired)

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

import org.junit.jupiter.api.Test;

public class InvalidTokenTest {

    // Base URL and Endpoint
    private static final String BASE_URL = "https://api.example.com";
    private static final String PROTECTED_ENDPOINT = "/protected-resource";

    @Test
    public void testInvalidToken() {
        // Use an invalid token (e.g., tampered or malformed)
        String invalidToken = "INVALID_OR_TAMPERED_TOKEN";

        given()
            .baseUri(BASE_URL)
            .header("Authorization", "Bearer " + invalidToken)
        .when()
            .get(PROTECTED_ENDPOINT)
        .then()
            .statusCode(401)  // Verify the status code is 401 Unauthorized
            .body("error", equalTo("Invalid token"));  // Verify the error message
    }
}



    
