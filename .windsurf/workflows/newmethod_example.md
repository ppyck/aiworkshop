---
description: Add a new method to an existing ABL class
package: business.logic
---

# New Method Addition Workflow

## Package: `business.logic`

This workflow is part of the `business.logic` package, which contains core business logic components for the application. Methods added using this workflow should follow the package's conventions and standards.

## Package-Level Guidelines

1. **Naming Conventions**
   - Use descriptive, action-oriented names (e.g., `CalculateTotal`, `ValidateOrder`)
   - Follow the pattern: `Verb` + `Noun` + `[Qualifier]`
   - Use PascalCase for method names

2. **Access Control**
   - Default to `PROTECTED` for internal methods
   - Use `PUBLIC` only for methods that are part of the public API
   - Mark implementation details as `PRIVATE`

3. **Documentation**
   - Include package name in method documentation
   - Reference related methods in the same package
   - Document any package-level assumptions or requirements

## Adding a New Method to the Current ABL Class

This workflow will add a new method to the currently focused ABL class in Windsurf. You'll need to provide the following information:

1. **Method Name**: Name of the new method
2. **Return Type**: Data type the method returns (use "VOID" if none)
3. **Access Modifier**: PUBLIC, PROTECTED, or PRIVATE
4. **Parameters**: Comma-separated list of parameters in format "name AS type, ..."
5. **Method Body**: The implementation code for the method

## Interactive Steps

1. **Determine Method Name**
   - Enter a descriptive name for your method (e.g., `GetCustomerName`, `CalculateTotal`)
   - Follow ABL naming conventions (PascalCase for method names)

2. **Define Return Type**
   - Specify the return type (e.g., `INTEGER`, `CHARACTER`, `DECIMAL`, `VOID`)

3. **Choose Access Modifier**
   - PUBLIC: Accessible from any code
   - PROTECTED: Accessible within the class and its subclasses
   - PRIVATE: Accessible only within the class

4. **Specify Parameters**
   - Provide parameters in the format: `name AS type`
   - Separate multiple parameters with commas

5. **Write Method Body**
   - Implement the intended behavior of the method
   - Include validation, calculations, or DB access as necessary

## Code Generation

Based on the provided information, the workflow will generate an ABL method template:

```abl
METHOD [ACCESS] [RETURN-TYPE] [MethodName]([Parameters]):
    /* Implementation */
END METHOD.
```

## Example

For a public method `GetCustomerName` that returns a character string and takes an integer customer ID:

1. Open the class file where you want to add the method in Windsurf
2. Run the `/newmethod` command
3. Provide the following details when prompted:
   - Method Name: `GetCustomerName`
   - Return Type: `CHARACTER`
   - Access Modifier: `PUBLIC`
   - Parameters: `piCustNum AS INTEGER`
   - Method Body:
     ```
     DEFINE VARIABLE cName AS CHARACTER NO-UNDO.
     
     FIND FIRST Customer NO-LOCK
          WHERE Customer.CustNum = piCustNum
          NO-ERROR.
          
     IF AVAILABLE Customer THEN
         cName = Customer.Name.
         
     RETURN cName.
     ```

## Example

For a method that retrieves a customer's name by ID:

```abl
/*------------------------------------------------------------------------------
    Purpose:  Returns the name of the customer with the specified ID
    Notes:    Returns an empty string if the customer is not found
    @param piCustomerId INTEGER - The ID of the customer to look up
    @return CHARACTER - The name of the customer or empty string if not found
------------------------------------------------------------------------------*/
METHOD PROTECTED CHARACTER GetCustomerName(piCustomerId AS INTEGER):
    
    DEFINE BUFFER bCustomer FOR Customer.
    DEFINE VARIABLE cName AS CHARACTER NO-UNDO.
    
    FIND FIRST bCustomer NO-LOCK
         WHERE bCustomer.CustNum = piCustomerId
         NO-ERROR.
         
    IF AVAILABLE bCustomer THEN
        cName = bCustomer.Name.
        
    RETURN cName.
    
END METHOD.
```
