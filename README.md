# Api-Core-DB-Schema

## Requirements
- **Database Server**: SQL Server or compatible (e.g., Azure SQL Edge).
- **Containerized Environment**: Deployable on Docker using [Azure SQL Edge](https://hub.docker.com/_/azure-sql-edge) (`mcr.microsoft.com/azure-sql-edge`).

## Schema Overview

### 1. Tables

- **Companies**: Stores company data.
- **Users**: Holds user information, including credentials and the associated company.
- **Roles**: Defines roles (e.g., Admin, User).
- **UserRoles**: Defines the many-to-many relationship between users and roles.

### Table Definitions

#### **Companies**
Stores company details.

| Column       | Type          | Description                              |
|--------------|---------------|------------------------------------------|
| Id           | INT           | Primary Key, unique identifier.          |
| CompanyName  | NVARCHAR(100) | Name of the company, required.           |
| CreatedAt    | DATETIME      | Defaults to current date and time.       |
| UpdatedAt    | DATETIME      | Defaults to current date and time.       |

#### **Users**
Stores user-specific information, including authentication details and associated company.

| Column       | Type          | Description                                        |
|--------------|---------------|----------------------------------------------------|
| Id           | INT           | Primary Key, unique identifier for each user.      |
| Username     | NVARCHAR(50)  | Unique username, required for authentication.      |
| PasswordHash | NVARCHAR(256) | Hashed password for secure authentication.         |
| Email        | NVARCHAR(100) | Unique email address.                              |
| CompanyId    | INT           | Foreign key to `Companies`, nullable on delete.    |
| CreatedAt    | DATETIME      | Defaults to current date and time.                 |
| UpdatedAt    | DATETIME      | Defaults to current date and time.                 |

#### **Roles**
Stores predefined roles, such as "Admin" and "User."

| Column       | Type          | Description                     |
|--------------|---------------|---------------------------------|
| Id           | INT           | Primary Key, unique identifier. |
| RoleName     | NVARCHAR(50)  | Unique name for each role.      |

#### **UserRoles**
Establishes a many-to-many relationship between `Users` and `Roles`.

| Column       | Type          | Description                                            |
|--------------|---------------|--------------------------------------------------------|
| UserId       | INT           | Foreign key to `Users`, part of the composite key.     |
| RoleId       | INT           | Foreign key to `Roles`, part of the composite key.     |
| AssignedAt   | DATETIME      | Timestamp for role assignment, defaults to now.        |
