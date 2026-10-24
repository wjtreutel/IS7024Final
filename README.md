# Cincy Food Safety Watch

**IS 7024 – Integration with Cloud Systems**  
**Group 1 – Project Design Document**

---

## Introduction

Cincy Food Safety Watch is a web application designed to help Cincinnati residents access restaurant food safety information in one place. The application combines public restaurant inspection records with Cincinnati 311 service request data to help users explore food safety concerns across different neighborhoods.

The goal of this project is to make it easier for people to find restaurant inspection information and learn about food-related complaints reported in their communities. Instead of searching through different city websites, users will be able to access this information through one application.

## Planned Features

- Search for restaurant inspection records.
- View available food safety violations.
- Explore food-related 311 complaints and service requests.
- Identify possible patterns in food safety complaints and restaurant inspections across Cincinnati neighborhoods.

---

## Data Sources

Our application will use two public JSON data sources from the City of Cincinnati Open Data Portal.

### 1. Cincinnati Food Safety Program

**JSON API:** [View Food Safety Data](https://data.cincinnati-oh.gov/resource/rg6p-b3h3.json)

This dataset contains information about food establishments and restaurant inspections in Cincinnati. It includes inspection dates, locations, inspection details, and recorded violations.

**How we will use it:**

We will use this data to allow users to search for restaurants and view available inspection information and food safety violations.

### 2. Cincinnati 311 (Non-Emergency) Service Requests

**JSON API:** [View Food-Related 311 Requests](https://data.cincinnati-oh.gov/resource/gcej-gmiw.json)

This dataset contains service requests submitted to the City of Cincinnati. It includes different types of complaints and requests, including some related to food establishments.

**How we will use it:**

We will search the dataset for food-related service requests. Users will be able to explore relevant requests and see which Cincinnati neighborhoods they were reported in.

**Note:** A 311 complaint does not automatically mean a restaurant violated food safety rules. Our application will display the two sources separately and only compare information where it makes sense.

---

## Functional Requirements

### Requirement 100.0 – Search Restaurant Inspection Records

#### User Story

As a Cincinnati resident, I want to search for restaurant inspection records so that I can check available food safety information before deciding where to eat.

#### Dependencies

- Cincinnati Food Safety Program API.
- An internet connection.
- A restaurant search page.

#### Assumptions

- The API is available.
- Restaurant names and inspection records are available through the Food Safety Program API.
- Multiple inspection records may exist for the same restaurant.
- Search results will be displayed at the restaurant level, with individual inspection records accessible through the restaurant details page.
- The application will handle missing records and API errors appropriately.

#### Examples

**Example 1 – Restaurant Found (Happy Path)**

- **Given** the dataset contains a restaurant named "Sample Bistro" with license reference `TEST-L001`,
- **When** I enter "Sample Bistro" in the restaurant search field and click Search,
- **Then** I should see "Sample Bistro" once in the search results, with an option to view its inspection history.

**Example 2 – No Matching Restaurant (Edge Case)**

- **Given** the dataset contains no restaurant matching "Nonexistent Test Restaurant",
- **When** I enter "Nonexistent Test Restaurant" and click Search,
- **Then** I should see the message "No matching restaurants found," with no restaurant records displayed.

**Example 3 – Restaurant With Multiple Inspections (Happy Path)**

- **Given** "Sample Bistro" has three inspection records dated January 15, March 10, and June 20, 2026, all associated with license `TEST-L001`,
- **When** I search for "Sample Bistro" and select the restaurant from the results,
- **Then** I should see one restaurant details page listing all three inspections, ordered from most recent to oldest.

**Example 4 – Blank Search Field (Happy Path)**

- **Given** the database contains 35 distinct restaurants,
- **When** I leave the restaurant search field blank and click Search,
- **Then** I should see all 35 restaurants across four pages, with 10 restaurants displayed on each of the first three pages and the remaining 5 on the fourth page.

**Example 5 – Partial and Case-Insensitive Search (Happy Path)**

- **Given** the dataset contains a restaurant named "Sample Bistro",
- **When** I enter "sample bis" in the restaurant search field and click Search,
- **Then** I should see "Sample Bistro" in the search results, regardless of capitalization.

**Example 6 – API Unavailable (Edge Case)**

- **Given** the Cincinnati Food Safety Program API returns an HTTP 503 Service Unavailable response and no cached restaurant records are available,
- **When** I search for "Sample Bistro",
- **Then** I should see the message "Restaurant inspection information is temporarily unavailable. Please try again later," rather than an unhandled system error.

---

### Requirement 200.0 – View Food Safety Violations

#### User Story

As a user, I want to view recorded food safety violations so that I can understand what was found during a restaurant inspection.

#### Dependencies

- Cincinnati Food Safety Program API.
- Restaurant inspection records.
- A page for displaying inspection details.

#### Assumptions

- Some inspections may have recorded violations.
- Not every inspection record will contain violation details.
- A restaurant may have multiple inspection records, each with its own associated violation information.
- A single inspection may contain multiple violations, which should be displayed under the corresponding inspection.
- Violation records may have missing or incomplete information, such as descriptions or comments.

#### Examples

**Example 1 – Inspection With Violations (Happy Path)**

- **Given** "Sample Bistro" has an inspection dated June 20, 2026, with three recorded violations,
- **When** I select that inspection from the restaurant details page,
- **Then** I should see the inspection date and all three violations, including their available violation codes and descriptions.

**Example 2 – No Violation Details Available (Edge Case)**

- **Given** "Sample Bistro" has an inspection dated March 10, 2026, with no violation details available in the dataset,
- **When** I select that inspection,
- **Then** I should see the inspection information and a message stating "No violation details available for this inspection."

**Example 3 – Multiple Violations Under One Inspection (Happy Path)**

- **Given** inspection `TEST-I001` has three distinct violation records associated with the same inspection identifier,
- **When** I view the inspection history for "Sample Bistro",
- **Then** I should see inspection `TEST-I001` listed only once, with all three violations accessible under that inspection.

**Example 4 – View Violations From a Previous Inspection (Happy Path)**

- **Given** "Sample Bistro" has two inspections, one dated January 15, 2026, with two violations and another dated June 20, 2026, with three violations,
- **When** I select the January 15 inspection,
- **Then** I should see only the two violations associated with that inspection, without displaying violations from the June 20 inspection.

**Example 5 – Missing Violation Description (Edge Case)**

- **Given** inspection `TEST-I001` has a violation record with code `TEST-V001` but no description,
- **When** I view the violation details,
- **Then** I should see the violation code and a message stating "Description not available," without omitting the violation record.

**Example 6 – Inspection Record Not Found (Edge Case)**

- **Given** inspection identifier `TEST-I999` does not exist in the available inspection records,
- **When** I attempt to open its inspection details page,
- **Then** I should see the message "Inspection record not found" instead of an unhandled application error.

---

### Requirement 300.0 – Explore Food-Related 311 Requests

#### User Story

As a Cincinnati resident, I want to view food-related 311 service requests in my neighborhood so that I can learn about concerns that have been reported to the city.

#### Dependencies

- Cincinnati 311 Service Requests API.
- A way to filter food-related requests.
- Available neighborhood information.

#### Assumptions

- Relevant food-related requests can be identified from the dataset.
- Some neighborhoods may have more records than others.
- Food-related requests will be identified using relevant service request types or descriptions available in the Cincinnati 311 dataset.
- Some service requests may have missing neighborhood information and may therefore be excluded from neighborhood-specific results.
- A reported 311 concern does not necessarily represent a confirmed food safety violation or establish a direct connection to a particular restaurant.

#### Examples

**Example 1 – Food-Related Requests Found (Happy Path)**

- **Given** the dataset contains five food-related 311 requests associated with the Over-the-Rhine neighborhood,
- **When** I select "Over-the-Rhine" on the 311 service requests page,
- **Then** I should see all five requests, including their available request types, submission dates, and statuses.

**Example 2 – No Food-Related Requests (Edge Case)**

- **Given** the dataset contains no food-related 311 requests associated with the selected neighborhood,
- **When** I select that neighborhood on the 311 service requests page,
- **Then** I should see the message "No matching food-related requests found," with no request records displayed.

**Example 3 – Exclude Unrelated Service Requests (Happy Path)**

- **Given** the Over-the-Rhine neighborhood has eight 311 requests, including three categorized as food-related and five categorized as unrelated issues such as potholes or street maintenance,
- **When** I select "Over-the-Rhine" on the food-related 311 requests page,
- **Then** I should see only the three food-related requests, excluding the five unrelated requests.

**Example 4 – Missing Neighborhood Information (Edge Case)**

- **Given** a food-related 311 request has no neighborhood information in the dataset,
- **When** I select a specific neighborhood to view its food-related requests,
- **Then** the request with missing neighborhood information should not appear in that neighborhood's results.

**Example 5 – View Request Status (Happy Path)**

- **Given** a food-related 311 request with reference `TEST-SR001` has a status of "Closed",
- **When** I view the request details,
- **Then** I should see its request reference, type, submission date, and "Closed" status, without the application presenting the status as confirmation of a food safety violation.

**Example 6 – API Unavailable (Edge Case)**

- **Given** the Cincinnati 311 Service Requests API is temporarily unavailable and no cached request records are available,
- **When** I select a neighborhood to view food-related requests,
- **Then** I should see the message "311 service request information is temporarily unavailable. Please try again later," instead of an unhandled system error.

---

### Requirement 400.0 – Compare Food Safety Information by Neighborhood

#### User Story

As a Cincinnati resident, I want to compare restaurant inspection information and food-related 311 requests by neighborhood so that I can better understand the available food safety information in different areas.

#### Dependencies

- Cincinnati Food Safety Program API.
- Cincinnati 311 Service Requests API.
- Neighborhood information from both datasets.

#### Assumptions

- Both datasets have information that can be grouped by neighborhood.
- Records in the two datasets may not always be directly related.
- Restaurant inspection records and food-related 311 requests will be grouped using the neighborhood information available in each dataset.
- A single restaurant inspection may have multiple violation records, but each inspection should be counted only once when calculating neighborhood inspection totals.
- Neighborhood comparisons will aggregate all available records by default, without requiring users to select a reporting period.
- The earliest and latest record dates will be displayed separately for each dataset to provide historical context.
- Food-related 311 requests represent reported concerns and will not automatically be attributed to specific restaurants or treated as confirmed food safety violations.
- Records without identifiable neighborhood information may be excluded from neighborhood-level comparisons.

#### Examples

**Example 1 – Both Data Sources Have Records (Happy Path)**

- **Given** the Over-the-Rhine neighborhood has four distinct restaurant inspections and seven food-related 311 requests in the available datasets,
- **When** I select "Over-the-Rhine" on the Neighborhood Insights page,
- **Then** I should see a neighborhood summary displaying four restaurant inspections and seven food-related 311 requests, with each total clearly identified by its data source.

**Example 2 – One Data Source Has No Records (Edge Case)**

- **Given** the selected neighborhood has five restaurant inspections but no matching food-related 311 requests,
- **When** I select that neighborhood on the Neighborhood Insights page,
- **Then** I should see five restaurant inspections and zero food-related 311 requests, with a message indicating that no matching 311 records were found.

**Example 3 – Prevent Duplicate Inspection Counts (Happy Path)**

- **Given** the Food Safety dataset contains 12 violation-level records associated with four distinct restaurant inspections in Over-the-Rhine,
- **When** I view the neighborhood's restaurant inspection summary,
- **Then** I should see a total of four inspections rather than 12, ensuring that inspections with multiple violations are not counted more than once.

**Example 4 – Compare Two Neighborhoods (Happy Path)**

- **Given** Over-the-Rhine has four restaurant inspections and seven food-related 311 requests, while Downtown has six inspections and three food-related 311 requests,
- **When** I compare "Over-the-Rhine" and "Downtown" on the Neighborhood Insights page,
- **Then** I should see the inspection and 311 request totals for both neighborhoods displayed side by side, with their respective sources clearly labeled.

**Example 5 – Display Historical Data Coverage (Happy Path)**

- **Given** the available inspection records for Over-the-Rhine span January 2018 to October 2026, while food-related 311 requests span March 2020 to October 2026,
- **When** I view the Over-the-Rhine neighborhood summary,
- **Then** I should see the total number of distinct inspections and food-related 311 requests, along with the earliest and latest record dates for each dataset.

**Example 6 – Missing Neighborhood Information (Edge Case)**

- **Given** the Food Safety dataset contains an inspection record without neighborhood information,
- **When** I view the summary for "Over-the-Rhine",
- **Then** the record should not be included in the Over-the-Rhine inspection total unless its neighborhood has been reliably identified.

**Example 7 – One Data Source Unavailable (Edge Case)**

- **Given** the Food Safety Program API is available but the Cincinnati 311 API is temporarily unavailable, with no cached 311 records available,
- **When** I view the neighborhood summary,
- **Then** I should see the available restaurant inspection information and a message indicating that 311 data is temporarily unavailable, rather than displaying a misleading zero request count.

**Optional Enhancement – Date-Range Filtering**

- As a potential future enhancement, users may be provided with an optional date-range filter to view and compare restaurant inspection records and food-related 311 requests within a specified reporting period.

---

## Icon / Logo

We plan to create a simple logo for Cincy Food Safety Watch that represents restaurant food safety.

![Placeholder Logo](https://github.com/wjtreutel/IS7024Final/blob/main/logo.png)

**Status:** Logo to be added.

---

## Storyboard / User Interface

Our proposed application will have the following pages:

1. **Home Page:** Introduces Cincy Food Safety Watch and explains what the application does.
2. **Restaurant Search Page:** Allows users to search for restaurant inspection records.
3. **Restaurant Details Page:** Displays inspection information and recorded violations.
4. **311 Requests Page:** Allows users to explore food-related service requests.
5. **Neighborhood Insights Page:** Shows available inspection and 311 information for a selected neighborhood.

### Visual Storyboard

The following storyboard illustrates the application's proposed main pages and user navigation.

- [View Storyboard (PDF)](docs/Cincy_Food_Safety_Storyboard.pdf)
- [View Editable Storyboard (PowerPoint)](docs/Cincy_Food_Safety_Storyboard.pptx)

**Note:** The storyboard illustrates the initial interface design. Additional details, including inspection selection and the presentation of associated violations, will be refined during development.

---

## Database

We plan to use a cloud database to store information needed by our application. Restaurant inspection and 311 data will be retrieved from the public JSON APIs.

### Proposed Tables

| Table Name | Proposed Fields | Purpose |
| --- | --- | --- |
| Neighborhood | NeighborhoodID (PK), NeighborhoodName | Stores neighborhood information used by the application. |
| FoodEstablishment | EstablishmentID (PK), SourceLicenseID, LicenseNumber, RestaurantName, Address, NeighborhoodID (FK, nullable) | Stores restaurant information and allows multiple inspections to be associated with the same establishment. |
| Inspection | InspectionID (PK), SourceInspectionID, EstablishmentID (FK), InspectionDate, InspectionType, InspectionStatus | Stores individual inspection records associated with each restaurant. |
| Violation | ViolationID (PK), SourceViolationKey, InspectionID (FK), ViolationCode, ViolationDescription, ViolationComments | Stores recorded food safety violations associated with individual inspections. |
| FoodServiceRequest | RequestID (PK), SourceRequestNumber, RequestType, RequestDate, RequestStatus, NeighborhoodID (FK, nullable) | Stores selected food-related 311 request information if needed. |

### Proposed Relationships

- One neighborhood can be connected to multiple food establishments.
- One food establishment can have multiple inspection records.
- One inspection can be associated with multiple food safety violations.
- One neighborhood can be connected to multiple food-related 311 service requests.
- NeighborhoodID will be used to associate establishments and service requests with their respective neighborhoods.
- NeighborhoodID may be null for establishments or service requests with missing neighborhood information. These records will be retained but excluded from neighborhood-level comparisons unless their neighborhood can be reliably identified.
- Restaurant inspections and 311 service requests will be aggregated separately by neighborhood for comparison. Each inspection will be counted only once, regardless of the number of associated violations.
- Food-related 311 requests will not be directly linked to individual restaurants or inspections, as a reported concern does not necessarily establish a connection to a specific restaurant.

**Note:** This is our initial database proposal. We will confirm the tables, fields, and relationships after reviewing the API data and deciding what information needs to be stored.

---

## GitHub Projects

We will use GitHub Projects to organize our tasks and track our progress.

### Planned Tasks

- [ ] Complete the README design document.
- [ ] Connect to the two JSON APIs.
- [ ] Create the restaurant search page.
- [ ] Create the inspection details page.
- [ ] Create the food-related 311 requests page.
- [ ] Create the neighborhood insights page.
- [ ] Set up the database.
- [ ] Test the application.
- [ ] Review the completed work.

We will use statuses such as **To Do**, **In Progress**, and **Done** to track our tasks.

**GitHub Projects Board:** To be added.

---

## Team Composition

### Group 1 Members

| Team Member | Role |
| --- | --- |
| Liam Treutel | To be decided by the group |
| Lila Isett | To be decided by the group |
| Comfort Okayi | To be decided by the group |
| Zarah May Zafra | To be decided by the group |
| Sean Dippold | To be decided by the group |


All five group members will contribute to the application. We will discuss and agree on our project roles and responsibilities.

---

## Weekly Team Meetings

**Meeting Platform:** Microsoft Teams

**Meeting Day:** To be confirmed by the group.

**Meeting Time:** To be confirmed by the group.

We currently communicate through our Microsoft Teams group chat. We will agree on a regular weekly meeting time to discuss project progress, review completed work, and plan our next tasks.

---

## GitHub Repository

**Original Group Repository:** [IS7024Final](https://github.com/wjtreutel/IS7024Final)

We will use GitHub to manage our project files, work on feature branches, and review changes through pull requests before merging them into the main branch.
