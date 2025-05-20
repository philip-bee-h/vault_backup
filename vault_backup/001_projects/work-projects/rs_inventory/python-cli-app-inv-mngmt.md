---
title: python-cli-app-inv-mngmt
created: 2023-10-27
modified: 2024-10-27
type: project
status: brainstorming
tags:
  - project
  - development
  - cli
  - database
  - sql
related_docs:
---

# Project Overview
Brainstorming and planning for the development of a simple Python command-line application (CLI) to manage an inventory database using Microsoft SQL.

# Objectives
- Develop a Python CLI app for managing inventory items within a Microsoft SQL database.
- Use this inventory as a source of truth across projects and align the solution with DevOps practices.

# Requirements
- **Database**: Microsoft SQL (provided by EI)
- **Programming Language**: Python
- **CLI Framework**: `Click` for building the command-line interface
- **Visualization**: Grafana for database visualization and reporting
- **Deployment**: Scriptable and integratable into various workflows

# Key Features
1. **Add Item**: Add new inventory items
2. **Update Item**: Modify existing inventory items
3. **Delete Item**: Remove items from the inventory
4. **Retrieve Item**: Retrieve information about specific items
5. **List Items**: List all inventory items

# Database Schema
A simple schema to structure inventory data. The primary inventory categories are listed with relevant fields.

## Inventory Table Schema Example
- **id** (Primary Key): Integer
- **name**: String
- **description**: Text
- **quantity**: Integer
- **location**: String
- **last_updated**: Timestamp

### Additional Tables
- **LSS Storage**:
  - **Fields**: `StorageID`, `ServerName`, `Location`, `RackPosition`, `PublicInterfaceFQDN`, `PrivateIP`, `MAC`, `FirewallSettings`, `WarrantyInfo`, etc.
- **HPC Inventory**:
  - **Fields**: `HPCID`, `Description`, `Location`, `Tickets`, `LastUpdated`, etc.
- **IDAS Inventory**:
  - **Fields**: `IDASID`, `SerialNumber`, `WarrantyInfo`, etc.

# Development Plan
### Step 1: Database Setup
- Set up Microsoft SQL database tables with the defined schema.
- Implement SQL scripts to initialize and structure the inventory tables.

### Step 2: CLI Development
- Use Python's `Click` framework for CLI structure.
- Sample Commands:
  - `inventory add --name "Item1" --description "Description" --quantity 10 --location "Warehouse"`
  - `inventory update --id 1 --quantity 15`
  - `inventory delete --id 1`
  - `inventory get --id 1`
  - `inventory list`

### Step 3: Database Integration
- Utilize `pyodbc` or `pymssql` to connect Python with the SQL database.
- Implement CRUD operations for the CLI.

### Step 4: Testing
- Develop unit tests using `unittest` or `pytest`.
- Integration tests to ensure database interactions function as expected.

### Step 5: Deployment
- Package the CLI for easy installation using `setuptools` or `poetry`.
- Document setup and usage.

# Integration and Automation
- Plan automation scripts that streamline inventory management tasks.
- Integrate with LSS and IDAS for enhanced cross-functional capabilities.

# Visualization
- Connect Grafana to the Microsoft SQL database.
- Set up dashboards to visualize metrics, track inventory changes, and generate reports.

# Documentation
- Outline comprehensive instructions for setup, usage, and CLI commands.
- Maintain a record of schemas, CLI commands, and integration points.

# Future Enhancements
- Potential web interface for broader access.
- Advanced search, filtering, and data categorization.
- Integration with DevOps tools and workflows.

