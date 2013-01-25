# Expense Tracking API

The Expense tracking API allows you to access and manipulate expense entries in similar fashion to using the weekly expensesheet view. This allows developers to create lightweight clients or widgets to track expenses beyond directly interacting with Harvest through the web browser.

## CREATE NEW EXPENSE

    POST /expenses

    HTTP Response: 201 Created
    Location: /expenses

Post the following for a standard expense with a total cost:

    <expense>
      <notes>Buy Valentine's Day chocolates for Harvest</notes>
      <total-cost type="decimal">11.00</total-cost>
      <project-id type="integer">2</project-id>
      <expense-category-id type="integer">1</expense-category-id>
      <spent-at type="date">Sun, 10 Feb 2008</spent-at>
    </expense>

Post the following for an expense whose total cost is calculated via an expense categories unit price (e.g. mileage):

    <expense>
      <notes>Drive to buy Valentine's chocolates</notes>
      <units type="integer">5</units>
      <project-id type="integer">2</project-id>
      <expense-category-id type="integer">3</expense-category-id>
      <spent-at type="date">Sun, 10 Feb 2008</spent-at>
    </expense>

## UPDATE AN EXISTING EXPENSE

    PUT /expenses/#{expense_id}

    HTTP Response: 200 OK 
    Location: /expenses/#{expense_id}

Put the following for a standard expense with a total cost:

    <expense>
      <notes>Buy Valentine's Day _dark_ chocolates for Harvest</notes>
      <total-cost type="decimal">20.00</total-cost>
    </expense>

Put the following for an expense whose total cost is calculated via an expense categories unit price (e.g. mileage):

    <expense>
      <notes>Drive _a long way_ to buy Valentine's chocolates</notes>
      <units type="integer">25</units>
    </expense>

## SHOW EXPENSE

    GET /expenses/#{expense_id}
    HTTP Response: 200 Success

    <expense>
      <notes>Buy Valentine's Day _dark_ chocolates for Harvest</notes>
      <total-cost type="decimal">20.00</total-cost>
      <project-id type="integer">2</project-id>
      <expense-category-id type="integer">1</expense-category-id>
      <spent-at type="date">Sun, 10 Feb 2008</spent-at>
      <has-receipt type="boolean">true</has-receipt>
      <receipt-url>https://sub.harvestapp.com/expenses/234997/receipt</receipt-url>
      <updated-at type="datetime">2008-04-09T12:07:56Z</updated-at>
      <created-at type="datetime">2008-04-09T12:07:56Z</created-at>
    </expense>

Note that the has-receipt field will indicate whether a receipt image has been attached. If it is true, you can use the URL in receipt-url to fetch the image.

## DELETE EXISTING EXPENSE

    DELETE /expenses/#{expense_id}

    HTTP Response: 200 OK 

## ATTACH A RECEIPT IMAGE TO AN EXPENSE

    POST /expenses/#{expense_id}/receipt

    HTTP Response: 200 OK 
    Location: /expenses/#{expense_id}/receipt

When adding or updating an image receipt, you don't need to post any XML. Just post the image data like you would any multipart form data. Be sure to set the name of the post data to expense[receipt] and set the filename= parameter:

    POST /expenses/#{expense_id}/receipt HTTP/1.1

    User-Agent: #{Your app name here}
    Host: #{yoursubdomain}.harvestapp.com
    Accept: application/xml
    Authorization: Basic (insert your authentication string here)
    Content-Length: 47899
    Content-Type: multipart/form-data; boundary=------------------------------b7edea381b46
------------------------------b7edea381b46
Content-Disposition: form-data; name="expense[receipt]"; filename="hotel.jpg"
    Content-Type: image/jpeg

    #{ BINARY IMAGE DATA }

    ------------------------------b7edea381b46

## GET A RECEIPT IMAGE ASSOCIATED WITH AN EXPENSE

    GET /expenses/#{expense_id}/receipt

    HTTP Response: 200 OK 

Perform a simple GET on the receipt URL to receive the image data.

## WORKING WITH EXPENSES FOR OTHER USERS

You may add an of_user=#{user_id} parameter to the URL of any expense tracking API call to work with the timesheet of another user. The authenticating user must be an administrator for this to work.

## WORKING WITH LOCKED EXPENSES

Administrators may edit and delete locked expenses. The following fields are considered readonly on locked expenses: project_id, expense_category_id, spent_at.