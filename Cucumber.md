# Interview Preparation
Question: Can you explain the difference between a Scenario Outline and a Data Table in Cucumber? How would you use each in a testing scenario?

Answer: 
Scenario Outline is used for running the same test with multiple sets of input data across different scenarios.
Data Table is used to pass complex data sets to a single step, typically for validating detailed information within a single scenario.

Use Case: Testing a patient appointment booking system where the same process needs to be tested with different patient details.

Feature File:
Feature: Patient Appointment Booking

  Scenario Outline: Book an appointment for different patients
    Given I am on the appointment booking page
    When I book an appointment for "<patientName>" with "<doctor>" on "<appointmentDate>"
    Then the appointment should be scheduled successfully

    Examples:
      | patientName | doctor       | appointmentDate |
      | John Doe    | Dr. Smith    | 2024-09-10      |
      | Jane Roe    | Dr. Johnson  | 2024-09-11      |
      | Mary Major  | Dr. Brown    | 2024-09-12      |

Step Definitions:
@When("I book an appointment for {string} with {string} on {string}")
public void bookAppointment(String patientName, String doctor, String appointmentDate) {
    // Code to book the appointment
    appointmentService.book(patientName, doctor, appointmentDate);
}

Usage: This scenario outline tests the booking functionality for different patients. It ensures the system handles various inputs correctly and schedules appointments as expected.

Data Table Example in Healthcare Industry:
Use Case: Verifying patient record details after an update in a hospital management system.

Feature: Verify Patient Record Details

  Scenario: Check updated patient record details
    Given I have updated the patient record for "John Doe"
    When I view the updated record
    Then I should see the following details:
     
      | field          | value            |
      | First Name     | John             |
      | Last Name      | Doe              |
      | Date of Birth  | 1980-01-01       |
      | Address        | 123 Elm St, City |
      | Phone Number   | 555-1234         |

Step Definitions:

@Then("I should see the following details:")

public void verifyPatientDetails(DataTable dataTable) {
   
    List<Map<String, String>> details = dataTable.asMaps(String.class, String.class);
    
    for (Map<String, String> row : details) {
        String field = row.get("field");
        String expectedValue = row.get("value");
        String actualValue = patientRecord.getField(field);
        assertEquals(expectedValue, actualValue, "Mismatch for " + field);
    
    }
}

Usage: The Data Table in this scenario is used to check that multiple fields in a patient's record match expected values after an update. This method is particularly useful for verifying detailed information in a structured way.

Question: How do you handle shared step definitions across multiple feature files in a large Cucumber project?

Answer: Organize shared step definitions into common or base classes to avoid duplication. Use package structures to group related steps logically, ensuring reusability across multiple feature files while maintaining a clean and scalable project structure.

Question: How do you use tags in Cucumber, and how would you run a specific set of tests using tags?

Answer: In Cucumber, tags are used to categorize scenarios or feature files, allowing you to selectively run specific tests. You can assign tags by placing @TagName above a scenario, feature, or step.

Question: How do you implement parameterization in Cucumber to handle dynamic data in your test cases?

Answer: Implement parameterization in Cucumber by using placeholders in step definitions with regular expressions or Cucumber expressions. This allows dynamic data input, enabling the same step to run with different data sets.

Question: How would you integrate Cucumber tests with Jenkins for continuous integration, and what challenges might you face?

Answer: Integrate Cucumber with Jenkins by configuring a Jenkins job to run Cucumber tests using Maven or Gradle. Challenges include managing dependencies, setting up test environments, handling parallel execution, and generating detailed test reports.

