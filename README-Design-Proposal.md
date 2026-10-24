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

**JSON API:** [View Food-Related 311 Requests](https://data.cincinnati-oh.gov/resource/gcej-gmiw.json?$q=food)

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
- The restaurant information is included in the dataset.

#### Examples

**Example 1 – Restaurant Found**

- **Given** I am on the restaurant search page,
- **When** I enter a restaurant name that exists in the records and click Search,
- **Then** I should see the available inspection records for that restaurant.

**Example 2 – No Matching Restaurant**

- **Given** I am on the restaurant search page,
- **When** I enter a restaurant name that has no matching records,
- **Then** I should see a message saying that no results were found.

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

#### Examples

**Example 1 – Inspection With Violations**

- **Given** I have searched for a restaurant,
- **When** I select an inspection record that has violations,
- **Then** I should see the inspection date and the available violation details.

**Example 2 – No Violation Details Available**

- **Given** I have selected an inspection record,
- **When** no violation details are available for that inspection,
- **Then** the application should display a message explaining that no violation details are listed in the available record.

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

#### Examples

**Example 1 – Food-Related Requests Found**

- **Given** I am on the 311 service requests page,
- **When** I select a neighborhood that has food-related requests,
- **Then** I should see the available requests and their details.

**Example 2 – No Food-Related Requests**

- **Given** I am on the 311 service requests page,
- **When** I select a neighborhood with no matching food-related requests,
- **Then** I should see a message that no matching records were found.

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

#### Examples

**Example 1 – Both Data Sources Have Records**

- **Given** I select a Cincinnati neighborhood,
- **When** inspection records and food-related 311 requests are available,
- **Then** I should see a summary of the available information from both sources.

**Example 2 – One Data Source Has No Records**

- **Given** I select a neighborhood,
- **When** only one dataset has matching records,
- **Then** the application should display the available information and show that no matching records were found in the other dataset.

---

## Icon / Logo

We plan to create a simple logo for Cincy Food Safety Watch that represents restaurant food safety.

**Status:** Logo to be added.

---

## Storyboard / User Interface

Our proposed application will have the following pages:

1. **Home Page:** Introduces Cincy Food Safety Watch and explains what the application does.
2. **Restaurant Search Page:** Allows users to search for restaurant inspection records.
3. **Inspection Details Page:** Displays inspection information and recorded violations.
4. **311 Requests Page:** Allows users to explore food-related service requests.
5. **Neighborhood Summary Page:** Shows available inspection and 311 information for a selected neighborhood.

A visual storyboard may be added as we continue planning the application.

---

## Database

We plan to use a cloud database to store information needed by our application. Restaurant inspection and 311 data will be retrieved from the public JSON APIs.

### Proposed Tables

| Table Name | Proposed Fields | Purpose |
| --- | --- | --- |
| Neighborhood | NeighborhoodID, NeighborhoodName | Stores neighborhood information used by the application. |
| RestaurantInspection | InspectionID, RestaurantName, InspectionDate, NeighborhoodID | Stores selected inspection information if needed. |
| FoodServiceRequest | RequestID, RequestType, RequestDate, NeighborhoodID | Stores selected food-related 311 request information if needed. |

### Proposed Relationships

- One neighborhood can be connected to multiple restaurant inspection records.
- One neighborhood can be connected to multiple food-related 311 service requests.
- NeighborhoodID would be used to connect the tables.

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
