# Bank Domain Model - Domain-Driven Design Analysis

## Task 1: Domain Objects Identification

### Bounded Contexts/Services
The bank application has been decomposed into four distinct bounded contexts, each representing a specific business domain:

1. **Customer Management Context** - Handles customer information and profile management
2. **Account Management Context** - Manages banking accounts and their operations
3. **Branch Management Context** - Handles branch-related information
4. **Card Management Context** - Manages ATM card services

### Aggregates
- **Account** (Abstract Aggregate Root) - Central aggregate that encapsulates account-related business logic and maintains consistency boundaries

### Entities
Entities are domain objects with unique identity that persist over time:

1. **Customer** (Customer Management Context)
   - Attributes: customerID, name, address, employmentDetails, accountNumbers

2. **SavingAccount** (Account Management Context) 
   - Inherits from Account
   - Additional Attributes: minimumBalance

3. **CurrentAccount** (Account Management Context)
   - Inherits from Account  
   - Additional Attributes: linkedSavingAccountNumber

4. **Branch** (Branch Management Context)
   - Attributes: branchName, address

5. **ATMCard** (Card Management Context)
   - Attributes: cardID, customerID, dateOfIssue, isActive

### Value Objects
Value objects are immutable objects defined by their attributes rather than identity:

1. **Name** (Customer Management Context)
   - Attributes: firstName, lastName

2. **Address** (Customer Management Context & Branch Management Context)
   - Attributes: street, city, dzongkhag, postalCode

3. **EmploymentDetails** (Customer Management Context)
   - Attributes: companyName, telephoneNumber, annualSalaryRange, occupation, designation

4. **Balance** (Account Management Context)
   - Attributes: amount, currency

5. **AccountType** (Account Management Context)
   - Attributes: type

6. **Date** (Card Management Context)
   - Attributes: day, month, year

## Task 2: Relationships and Associations

### Within Bounded Contexts

#### Customer Management Context
- **Customer** has a **Name** (1:1 composition)
- **Customer** has an **Address** (1:1 composition) 
- **Customer** has **EmploymentDetails** (1:1 composition)

#### Account Management Context
- **Account** (abstract) is specialized by **SavingAccount** and **CurrentAccount** (inheritance)
- **Account** has a **Balance** (1:1 composition)
- **Account** is of an **AccountType** (1:1 composition)
- **SavingAccount** has a minimum **Balance** (1:1 composition)

#### Branch Management Context
- **Branch** is located at an **Address** (1:1 composition)

#### Card Management Context
- **ATMCard** is issued on a **Date** (1:1 composition)

### Cross-Context Relationships

#### Customer-Account Relationship (Cross-Context)
- **Customer** can be primary holder of multiple **Accounts** (1:*)
- **Customer** can be joint holder of multiple **Accounts** (0:*)
- **Account** can have maximum 2 holders (1 primary + 1 optional joint)

#### Customer-ATMCard Relationship (Cross-Context)
- **Customer** can have at most one **ATMCard** (1:0..1)
- **ATMCard** belongs to exactly one **Customer** (1:1)

#### Account-Branch Relationship (Cross-Context)
- **Account** belongs to exactly one **Branch** (*:1)
- **Branch** does not maintain references to **Accounts** (unidirectional relationship)

#### CurrentAccount-SavingAccount Relationship (Within Context)
- **CurrentAccount** can be linked to one **SavingAccount** (0..1:1)

### Key Relationship Characteristics

1. **Joint Account Support**: The model supports joint accounts where a customer can be either a primary holder (mandatory) or joint holder (optional) of an account.

2. **Account Independence**: When an account is closed, the customer record remains active, demonstrating loose coupling between customer and account lifecycles.

3. **ATM Card Optionality**: Customers can exist without ATM cards, and ATM card termination doesn't affect customer records.

4. **Branch Association**: Accounts are associated with branches, but branches don't maintain backward references to accounts, following the specified business rule.

5. **Account Linking**: Current accounts can optionally link to saving accounts for overdraft protection or automatic transfers.

### Multiplicity Summary
- Customer → Account (Primary): 1 to many (1:*)
- Customer → Account (Joint): 0 to many (0:*)  
- Customer → ATMCard: 0 to 1 (0..1:1)
- Account → Branch: many to 1 (*:1)
- CurrentAccount → SavingAccount: 0 to 1 (0..1:1)
- Entity → Value Object: 1 to 1 (composition)