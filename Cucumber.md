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

Answer: Use Case: Managing Patient Records and Appointments

Scenario: In a healthcare management system, you have multiple features related to patient management and appointment scheduling. You need to ensure that common steps used in various scenarios, such as verifying patient information and booking appointments, are reused efficiently.

Feature File 1: Patient Management

Feature: Update Patient Information

  Scenario: Update patient contact details
    
    Given I have a patient record for "Alice Johnson"
    
    When I update the contact details to "555-6789"
    
    Then the updated contact details should be "555-6789"

  Scenario: Update patient address
    
    Given I have a patient record for "Alice Johnson"
    
    When I update the address to "456 Maple Ave, Springfield"
    
    Then the updated address should be "456 Maple Ave, Springfield"


 Feature File 2: Appointment Scheduling

Feature: Schedule Appointments

  Scenario: Book an appointment for a patient

    Given I am on the appointment scheduling page
    
    When I book an appointment for "Alice Johnson" with "Dr. Lee" on "2024-10-01"
    
    Then the appointment confirmation should display:
     
      | patientName | doctor   | appointmentDate |
      | Alice Johnson | Dr. Lee | 2024-10-01      |

  Scenario: Cancel an existing appointment
    
    Given I have an appointment for "Alice Johnson" with "Dr. Lee" on "2024-10-01"
    
    When I cancel the appointment
    
    Then the appointment should no longer be listed

Shared Step Definitions
Common Step Definitions File (e.g., CommonSteps.java)

@Given("I have a patient record for {string}")

public void loadPatientRecord(String patientName) {

    // Code to retrieve or create a patient record
    
    patientRecord = patientService.getPatientByName(patientName);
}

@When("I update the contact details to {string}")

public void updateContactDetails(String newContact) {

    patientRecord.setContactNumber(newContact);
    
    patientService.savePatient(patientRecord);
}

@Then("the updated contact details should be {string}")

public void verifyUpdatedContactDetails(String expectedContact) {
  
    assertEquals(expectedContact, patientRecord.getContactNumber());
}

@When("I update the address to {string}")

public void updateAddress(String newAddress) {

    patientRecord.setAddress(newAddress);
    
    patientService.savePatient(patientRecord);
}

@Then("the updated address should be {string}")

public void verifyUpdatedAddress(String expectedAddress) {

    assertEquals(expectedAddress, patientRecord.getAddress());
}

@Given("I am on the appointment scheduling page")

public void navigateToSchedulingPage() {

    // Code to navigate to the scheduling page
}

@When("I book an appointment for {string} with {string} on {string}")

public void bookAppointment(String patientName, String doctor, String appointmentDate) {
    
    // Code to book an appointment

    appointmentService.bookAppointment(patientName, doctor, appointmentDate);
}

@Then("the appointment confirmation should display:")

public void verifyAppointmentConfirmation(DataTable dataTable) {

    List<Map<String, String>> appointmentDetails = dataTable.asMaps(String.class, String.class);
    
    // Code to verify appointment details
    
    for (Map<String, String> details : appointmentDetails) {
    
        assertEquals(details.get("patientName"), appointment.getPatientName());
        
        assertEquals(details.get("doctor"), appointment.getDoctor());
        
        assertEquals(details.get("appointmentDate"), appointment.getDate());
    }
}

@When("I cancel the appointment")

public void cancelAppointment() {

    // Code to cancel the appointment
    
    appointmentService.cancelAppointment(patientName, doctor, appointmentDate);
}

@Then("the appointment should no longer be listed")

public void verifyAppointmentCancellation() {

    // Code to check that the appointment is no longer listed
    
    assertFalse(appointmentService.isAppointmentListed(patientName, doctor, appointmentDate));
}

Passing data from one step to another in Cucumber can be done using various techniques, depending on the language and framework you're working with. One common approach is to use instance variables within the step definition class or context classes to store data that can be accessed by subsequent steps.

Feature: Patient Management

  Scenario: Create and verify patient record
    
    When I create a new patient with name "John Doe" and age "30"
    
    Then I should see the patient record with name "John Doe" and age "30"

@When("I create a new patient with name {string} and age {int}")
    
    public void createNewPatient(String name, int age) {
    
        // Code to create a new patient
        
        this.patientName = name;  // Store data to be used in the next step
        
        this.patientAge = age;
        
        // Assume that there is a service or method to create a patient
        patientService.createPatient(name, age);
    }

Instance Variables: Data can be stored in instance variables of the step definition class and accessed by subsequent steps.

Context Classes: In larger projects, you might use a context or state class to hold the data across steps, especially when dealing with more complex data or multiple feature files.

A context class typically holds all the data that needs to be shared among the steps in a scenario. Each scenario has its own instance of the context class, ensuring that data is isolated between scenarios, which avoids side effects and allows for parallel execution.

Step 1: Create a Context Class

Step 2: Modify the Step Definitions to Use the Context Class

Step 3: Configure Dependency Injection

TestContext.java:

public class TestContext {
    private Patient patient;
    private Appointment appointment;

    public Patient getPatient() {
        return patient;
    }

    public void setPatient(Patient patient) {
        this.patient = patient;
    }

    public Appointment getAppointment() {
        return appointment;
    }

    public void setAppointment(Appointment appointment) {
        this.appointment = appointment;
    }
}

public class PatientSteps {

    private TestContext context;

    public PatientSteps(TestContext context) {
        this.context = context;
    }

    

Question: How do you use tags in Cucumber, and how would you run a specific set of tests using tags?

In Cucumber, tags are a powerful feature that allow you to group and selectively run specific scenarios or feature files. This can be particularly useful in a large project, such as in the healthcare industry, where you might have a wide range of tests covering different aspects of the application, such as patient management, appointment scheduling, billing, and more.

Using Tags in Cucumber
1. Tagging Scenarios and Features:

You can tag individual scenarios or entire feature files by placing tags above the Feature, Scenario, or Scenario Outline keyword. Tags are prefixed with @ and can be any name you choose.

@PatientManagement
Feature: Manage patient records

  @CreatePatient
  Scenario: Create a new patient record
    Given I am on the patient registration page
    When I enter the patient's details
    Then a new patient record should be created

  @UpdatePatient
  Scenario: Update an existing patient record
    Given I have a patient record
    When I update the patient's contact information
    Then the patient record should reflect the updated information

@AppointmentBooking
Feature: Book appointments

  @BookAppointment
  Scenario: Book an appointment for a patient
    Given a patient is registered
    When I book an appointment with Dr. Smith on "2024-09-01"
    Then the appointment should be scheduled successfully

2. Running Specific Tagged Scenarios:

Using Maven:

mvn test -Dcucumber.options="--tags @PatientManagement"

Combining Tags:

AND logic: Run scenarios that have both tags.

mvn test -Dcucumber.options="--tags @PatientManagement and @CreatePatient"

OR logic: Run scenarios that have either tag.

mvn test -Dcucumber.options="--tags @CreatePatient,@UpdatePatient"


Question: How do you implement parameterization in Cucumber to handle dynamic data in your test cases?

parameterization allows you to handle a wide range of dynamic data, from patient records to appointment scheduling and billing. By using Scenario Outline, Data Tables, and Cucumber Expressions, you can create flexible and reusable test cases

1. Cucumber Expressions
 
 @Given("I register a patient named {string} who is {int} years old")

 2. Using Data Tables for Complex Parameterization:
 
    For more complex data, such as a patient's full record or a series of medical tests, you can use Data Tables to parameterize the steps.

3. Scenario Outline with Examples:

   Using Scenario Outline with Examples is a common way to parameterize test cases. This allows you to define a scenario template and then provide different sets of data to be tested.

Question: How would you integrate Cucumber tests with Jenkins for continuous integration, and what challenges might you face?

Step-by-Step Approach

Step 1: Install Jenkins and Set Up the Project

Create a Jenkins Job:

Log in to Jenkins.
Click on "New Item" to create a new job.
Choose "Freestyle project" or "Pipeline" (depending on your preference).
Name your project and click "OK".

Step 2: Configure Source Code Management
Connect to Your Source Code Repository:
In the job configuration page, under "Source Code Management," select "Git".
Provide the repository URL and credentials (if needed).
Specify the branch you want to build, typically main or master.

Step 3: Add Build Steps to Run Cucumber Tests
  1. Add a Build Step for Maven/Gradle:

If your project uses Maven, add a "Invoke top-level Maven targets" build step.
2. Configure the Build Step:

clean test
3. Ensure Proper Plugin Installation:

Install the Cucumber Reports plugin in Jenkins to generate Cucumber HTML reports. Go to "Manage Jenkins" → "Manage Plugins" → "Available", search for "Cucumber Reports", and install it.


Step 4: Post-Build Actions for Cucumber Reports
Configure Cucumber Reports:

Add a "Publish Cucumber test result report" post-build action.
Set the JSON file path, typically: target/cucumber.json (for Maven) or build/reports/cucumber.json (for Gradle).
Set Up Email Notifications:

You can also configure Jenkins to send email notifications when tests fail or pass, by adding a "E-mail Notification" post-build action.

Step 5: Trigger the Build
Trigger the Build Automatically:
Configure triggers under "Build Triggers" in Jenkins. Common options include:
Poll SCM: Jenkins periodically checks for changes in the repository.
Build when a change is pushed to GitHub: Use GitHub webhooks to trigger builds on every push.
Save the configuration.

Step 6: Review and Monitor the Test Execution
Monitor Build Execution:

Click "Build Now" to run the job manually or wait for an automatic trigger.
Check the console output in Jenkins to monitor the progress.
Review Cucumber Reports:

After the build, navigate to the "Cucumber Reports" section to view detailed reports, including passed and failed scenarios, along with step definitions.
