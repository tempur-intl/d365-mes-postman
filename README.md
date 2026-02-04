# D365 MES Integration Postman Collection

This Postman collection provides requests for interacting with Dynamics 365 Supply Chain Management Manufacturing Execution System (MES) Integration API. It includes basic request templates with essential fields that can be expanded to support all documented fields.

## Features

- **Basic Request Templates**: Pre-configured requests for all MES message types with essential fields
- **Comprehensive Variables**: 70+ variables covering all possible fields from the Microsoft documentation
- **OAuth Authentication**: Client credentials flow with automatic token storage
- **Environment Support**: Separate environments for Development and UAT

## Setup

1. Import the collection: `D365_MES_Integration.postman_collection.json`
2. Import the environments:
   - `D365_Dev.postman_environment.json` for Development
   - `D365_UAT.postman_environment.json` for UAT

3. Update the environment variables with your actual values:
   - `tenant_id`: Your Azure AD tenant ID
   - `client_id`: Application (client) ID from Azure AD app registration
   - `client_secret`: Client secret from Azure AD app registration
   - `company_id`: Company ID in D365 (e.g., "500")

## Usage

1. Select the appropriate environment (Dev or UAT)
2. Run the "Get OAuth Token" request to obtain an access token
3. Adjust the collection variables as needed for your test data
4. For each request, modify the raw body to include additional fields as needed

## Variables

The collection includes **70+ variables** covering all possible fields from the Microsoft documentation. Key categories:

- **Basic**: `prod_order`, `company_id`, dates, quantities
- **Consumption Rules**: `bom_rule`, `route_rule`
- **Journal Settings**: `journal_name`, `journal_description`, `posting_date`, `document_date`
- **Inventory Dimensions**: 12 dimensions for tracking inventory attributes
- **Product Attributes**: 10 custom product attributes
- **Operation Details**: Priority, description, resource information, timing
- **Quality Control**: Good/error/scrap quantities, error causes
- **Worker & Equipment**: Worker, shift, team, machine, tool information
- **Custom Properties**: 10 additional custom properties

Optional fields default to empty strings or "No". All variables are pre-configured in the collection and can be modified as needed.

## Customizing Requests

To include all fields in a request:

1. Edit the request's raw body
2. Expand the `_messageContent` JSON string to include additional fields using the collection variables
3. For example, for Report as Finished, add `"PrintLabel": "{{print_label}}"` and other fields as needed

The variables are available for all documented fields, allowing full customization of payloads.

## Notes

- The MES integration processes messages asynchronously (every 1 minute)
- Check the Manufacturing execution systems integration page in D365 for message status
- Ensure the Time and attendance license key is enabled in D365

## Reference

Based on: https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/mes-integration