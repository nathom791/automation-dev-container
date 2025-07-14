Feature: User Authentication

  Background:
    Given the user is on the Saucedemo login page

  Scenario Outline: Log in with valid and invalid credentials
    When the user enters username "<username>" and password "<password>"
    And the user clicks the Login button
    Then the login outcome should be "<outcome>"

    *Get the usernames and passwords from the bitwarden mcp server for all logins that are associated with saucedemo.com
  Scenario: Log out
    Given the user is logged in as standard_user
    When the user opens the side menu
    And the user clicks Logout
    Then the user should be redirected to the login page


Feature: Product Listing & Filtering

  Background:
    Given the user is logged in as standard_user
    And the user is on the Products page

  Scenario: Default product sorting
    Then all products are displayed sorted by Name (A to Z) by default

  Scenario: Change sorting to Price (high to low)
    When the user selects the "Price (high to low)" filter
    Then products are reordered from highest price to lowest price


Feature: Shopping Cart Management

  Background:
    Given the user is logged in as standard_user
    And the user is on the Products page

  Scenario: Add a single item to the cart
    When the user adds the "Sauce Labs Backpack" to the cart
    Then the cart badge shows "1"
    And the cart page contains "Sauce Labs Backpack"

  Scenario: Add multiple items to the cart
    When the user adds the following items to the cart:
      | item name             |
      | Sauce Labs Backpack   |
      | Sauce Labs Bike Light |
    Then the cart badge shows "2"
    And the cart page lists both items

  Scenario: Remove an item from the cart
    Given the cart contains "Sauce Labs Backpack" and "Sauce Labs Bike Light"
    When the user removes "Sauce Labs Backpack"
    Then the cart badge shows "1"
    And the cart page contains only "Sauce Labs Bike Light"

  Scenario: View empty cart
    Given the cart is empty
    When the user navigates to the cart page
    Then the cart page displays "Your cart is empty"


Feature: Checkout Process

  Background:
    Given the user is logged in as standard_user
    And the cart contains "Sauce Labs Backpack"
    And the user is on the Cart page

  Scenario: Complete checkout with valid information
    When the user clicks Checkout
    And the user fills in First Name "Alice", Last Name "Smith", Postal Code "12345"
    And the user clicks Continue
    And the user clicks Finish
    Then the user sees the message "THANK YOU FOR YOUR ORDER"

  Scenario: Checkout missing required fields
    When the user clicks Checkout
    And the user leaves First Name blank
    And the user fills in Last Name "Smith" and Postal Code "12345"
    And the user clicks Continue
    Then the user sees error "Error: First Name is required"


Feature: Error Handling & Resilience

  Background:
    Given the user is on the Saucedemo login page

  Scenario: Network interruption during login
    When the network connection is lost
    And the user attempts to log in with valid credentials
    Then the user sees a network error message

  Scenario: Slow‐loading inventory (performance_glitch_user)
    Given the user logs in as performance_glitch_user
    When the inventory takes longer than 10 seconds to load
    Then a loading spinner is displayed until products appear
