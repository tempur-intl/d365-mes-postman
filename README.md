# D365 MES Collections

This repository contains Postman collections for testing Dynamics 365 Supply Chain Management Manufacturing Execution System (MES) related services.

## Collections Overview

### 1. D365 MES Integration Collection
**File**: `D365_MES_Integration.postman_collection.json`

Provides requests for interacting with the Dynamics 365 MES Integration API (message-based) and OData endpoints for retrieving data from TSI custom entities and standard D365 entities.

**Features:**
- **MES Message API**: Pre-configured requests for production order operations (Start, Report Finished, Material Consumption, etc.)
- **OData Endpoints**: Queries for TSI custom entities and standard D365 entities (WarehouseWorkLines, ItemBatches)
- **Comprehensive Variables**: 70+ variables covering all possible fields
- **OAuth Authentication**: Client credentials flow with automatic token storage
- **Environment Support**: Separate environments for Development and UAT

### 2. Inventory Visibility Service Collection
**File**: `Inventory_Visibility.postman_collection.json`

Provides requests for testing the Dynamics 365 Inventory Visibility Service API.

**Features:**
- **AAD Token Authentication**: Client credentials flow to obtain Azure AD access token
- **IV Bearer Token**: Exchange AAD token for Inventory Visibility service bearer token
- **Index Query**: Query inventory on-hand data with filtering and grouping capabilities
- **Environment Support**: Separate environments for UAT and Production

## Setup

### D365 MES Integration Collection

1. Import the collection: `D365_MES_Integration.postman_collection.json`
2. Import the environments:
   - `D365_Dev.postman_environment.json` for Development
   - `D365_UAT.postman_environment.json` for UAT

3. Update the environment variables with your actual values:
   - `tenant_id`: Your Azure AD tenant ID
   - `client_id`: Application (client) ID from Azure AD app registration
   - `client_secret`: Client secret from Azure AD app registration
   - `company_id`: Company ID in D365 (e.g., "500")

### Inventory Visibility Service Collection

1. Import the collection: `Inventory_Visibility.postman_collection.json`
2. Import the environments:
   - `Inventory_Visibility_UAT.postman_environment.json` for UAT
   - `Inventory_Visibility_Production.postman_environment.json` for Production

3. Update the environment variables with your actual values:
   - `tenant_id`: Your Azure AD tenant ID
   - `client_id`: Application (client) ID from Azure AD app registration
   - `client_secret`: Client secret from Azure AD app registration
   - `aad_scope`: Azure AD app scope (format: `app-id/.default`)
   - `iv_scope`: Inventory Visibility service scope (usually `https://inventoryservice.operations365.dynamics.com/.default`)
   - `environment_id`: Your D365 environment ID
   - `context_type`: Usually "finops-env"
   - `iv_base_url`: Inventory Visibility service base URL (region-specific)
   - Sample filter values: `product_id`, `site_id`, `location_id`, `organization_id`

## Usage

### D365 MES Integration

1. Select the appropriate environment (Dev or UAT)
2. Run the "Get OAuth Token" request to obtain an access token
3. Adjust the collection variables as needed for your test data
4. For MES requests, modify the raw body to include additional fields as needed
5. For OData requests, adjust filter variables as needed

### Inventory Visibility Service

1. Select the appropriate environment (UAT or Production)
2. Run the "Get AAD Token for IV" request to obtain an Azure AD access token
3. Run the "Get Bearer token using access token" request to exchange for IV service token
4. Run the "IV Index Query" request to query inventory data
5. Adjust filter variables as needed for your test data

## MES Integration Details

### MES Message API Requests

The collection includes requests for all standard MES message types:
- Start Production Order
- Report as Finished
- Material Consumption (Picking List)
- Time Used for Operation (Route Card)
- End Production Order

### OData Endpoints

The collection includes OData queries for TSI custom entities and standard D365 entities using $filter syntax:

- **Get TSI_Items**: `?$filter=dataAreaId eq '{{company_id}}' and ItemId eq '{{item_id}}'`
- **Get TSI_JmgJobs**: `?$filter=dataAreaId eq '{{company_id}}' and ProdId eq '{{prod_id}}'`
- **Get TSI_Labels**: `?$filter=dataAreaId eq '{{company_id}}' and ProdId eq '{{prod_id}}'&$expand=Logos`
- **Get TSI_LabelLogos**: Retrieves all label logos data
- **Get TSI_ProdBOMLines**: `?$filter=dataAreaId eq '{{company_id}}' and ProdId eq '{{prod_id}}'`
- **Get WarehouseWorkLines**: `?$filter=dataAreaId eq '{{company_id}}' and WarehouseWorkStatus ne Microsoft.Dynamics.DataEntities.WHSWorkStatus'Closed' and WarehouseWorkStatus ne Microsoft.Dynamics.DataEntities.WHSWorkStatus'Cancelled'&$select=LicensePlateNumber,WarehouseWorkStatus,WarehouseWorkId,ItemNumber`
- **Get ItemBatches**: `?$filter=dataAreaId eq '{{company_id}}' and BatchDispositionCode eq '{{disposition_code}}'&$select=ItemNumber,BatchNumber,BatchDispositionCode,ManufacturingDate,BatchExpirationDate`

These endpoints use the same authentication as the MES requests. The variables `item_id`, `prod_id`, and `disposition_code` are available for customization.

### Variables

The MES collection includes **70+ variables** covering all possible fields from the Microsoft documentation. Key categories:

- **Basic**: `prod_order`, `company_id`, dates, quantities
- **Consumption Rules**: `bom_rule`, `route_rule`
- **Journal Settings**: `journal_name`, `journal_description`, `posting_date`, `document_date`
- **Inventory Dimensions**: 12 dimensions for tracking inventory attributes
- **Product Attributes**: 10 custom product attributes
- **Operation Details**: Priority, description, resource information, timing
- **Quality Control**: Good/error/scrap quantities, error causes
- **Worker & Equipment**: Worker, shift, team, machine, tool information
- **Custom Properties**: 10 additional custom properties

## Inventory Visibility Details

### Requests

#### Get AAD Token for IV
Obtains an Azure AD access token using client credentials flow.

#### Get Bearer token using access token
Exchanges the AAD token for an Inventory Visibility service bearer token using the security service.

#### IV Index Query
Queries inventory on-hand data with the following features:
- Filters by Product ID, Site ID, Location ID, and Organization ID
- Groups results by Batch ID, Location ID, WMS Location ID, and License Plate ID
- Returns negative quantities
- Includes visualizer for formatted results display

### Variables

The Inventory Visibility collection uses environment variables for all configuration. No hardcoded values remain in the collection.

## Notes

### MES Integration
- The MES integration processes messages asynchronously (every 1 minute)
- Check the Manufacturing execution systems integration page in D365 for message status
- Ensure the Time and attendance license key is enabled in D365

### Inventory Visibility
- The Inventory Visibility service requires proper Azure AD app registration and permissions
- Environment IDs and service URLs are region-specific
- The index query supports additional filters and grouping options not included in this basic template

## References

- **MES Integration**: https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/mes-integration
- **Inventory Visibility**: https://learn.microsoft.com/en-us/dynamics365/supply-chain/inventory/inventory-visibility-api