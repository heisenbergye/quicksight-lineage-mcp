# QuickSight Lineage MCP Server

A Model Context Protocol (MCP) server that provides comprehensive lineage analysis and resource management capabilities for Amazon QuickSight.

## Overview

This MCP server enables you to analyze data lineage relationships across QuickSight resources including dashboards, analyses, datasets, and data sources. It helps you understand dependencies, track data flow, and manage your QuickSight environment effectively.

## Features

### Resource Listing
- **List Dashboards**: Get all dashboards in your QuickSight account
- **List Analyses**: Retrieve all analyses with their IDs and names
- **List Datasets**: View all available datasets
- **List Data Sources**: Access all configured data sources

### Lineage Analysis
- **Dashboard Lineage**: Analyze which analysis and datasets a dashboard depends on
- **Analysis Lineage**: Understand the datasets and data sources used by an analysis
- **Dataset Lineage**: Track the data sources that feed into a dataset
- **Data Source Lineage**: View downstream dependencies of data sources

### Account Overview
- **QuickSight Overview**: Get comprehensive statistics about your QuickSight resources

## Available Tools

### Resource Listing Tools

#### `list_dashboards`
Lists all dashboards in the specified AWS account and region.
```
Parameters:
- account_id (required): AWS account ID
- region (optional): AWS region (default: us-east-1)
```

#### `list_analyses`
Lists all analyses in the specified AWS account and region.
```
Parameters:
- account_id (required): AWS account ID
- region (optional): AWS region (default: us-east-1)
```

#### `list_datasets`
Lists all datasets in the specified AWS account and region.
```
Parameters:
- account_id (required): AWS account ID
- region (optional): AWS region (default: us-east-1)
```

#### `list_datasources`
Lists all data sources in the specified AWS account and region.
```
Parameters:
- account_id (required): AWS account ID
- region (optional): AWS region (default: us-east-1)
```

### Lineage Analysis Tools

#### `analyze_dashboard`
Analyzes the lineage of a specific dashboard, showing its source analysis and datasets.
```
Parameters:
- account_id (required): AWS account ID
- dashboard_id (required): QuickSight dashboard ID
- region (optional): AWS region (default: us-east-1)
```

#### `analyze_analysis`
Analyzes the lineage of a specific analysis, showing its datasets and data sources.
```
Parameters:
- account_id (required): AWS account ID
- analysis_id (required): QuickSight analysis ID
- region (optional): AWS region (default: us-east-1)
```

#### `analyze_dataset`
Analyzes the lineage of a specific dataset, showing its data sources.
```
Parameters:
- account_id (required): AWS account ID
- dataset_id (required): QuickSight dataset ID
- region (optional): AWS region (default: us-east-1)
```

#### `analyze_datasource`
Analyzes the lineage of a specific data source, showing its downstream dependencies.
```
Parameters:
- account_id (required): AWS account ID
- datasource_id (required): QuickSight data source ID
- region (optional): AWS region (default: us-east-1)
```

### Overview Tool

#### `quicksight_overview`
Provides a comprehensive overview of QuickSight resources in the account.
```
Parameters:
- account_id (required): AWS account ID
- region (optional): AWS region (default: us-east-1)
```

## Usage Examples

### Basic Resource Listing
```bash
# List all dashboards
q chat "List all QuickSight dashboards in account 123456789012"

# List analyses in a specific region
q chat "Show me all analyses in us-west-2 for account 123456789012"
```

### Lineage Analysis
```bash
# Analyze dashboard lineage
q chat "Analyze the lineage for dashboard f0449be7-d2e6-4b9d-b405-3a93c08f7f05 in account 808577411626"

# Find which analysis a dashboard uses
q chat "What analysis does dashboard abc123 correspond to?"

# Trace dataset dependencies
q chat "Show me the data sources used by dataset xyz789"
```

### Account Overview
```bash
# Get comprehensive QuickSight statistics
q chat "Give me an overview of QuickSight resources in account 123456789012"
```

## Prerequisites

- AWS credentials configured with appropriate QuickSight permissions
- Access to the target AWS account and regions
- QuickSight service activated in the target regions

## Required AWS Permissions

The MCP server requires the following QuickSight permissions:
- `quicksight:ListDashboards`
- `quicksight:ListAnalyses`
- `quicksight:ListDataSets`
- `quicksight:ListDataSources`
- `quicksight:DescribeDashboard`
- `quicksight:DescribeAnalysis`
- `quicksight:DescribeDataSet`
- `quicksight:DescribeDataSource`

## Installation and Configuration

1. Install UV on macOS/Linux
```
curl -LsSf https://astral.sh/uv/install.sh | sh
```
2. create venv and 
```
% git clone https://github.com/heisenbergye/quicksight-lineage-mcp.git
% cd quicksight-lineage-mcp
% uv venv .venv
% source .venv/bin/activate
% uv add "mcp[cli]" boto3
```
3. [Install Q CLI](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-installing.html) and [Ensure the MCP server is properly configured in your Q CLI environment](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-mcp-understanding-config.html), or you can use CLINE in IDE etc.
```
$ cat ~/.aws/amazonq/mcp.json
{
  "mcpServers": {
    "quicksight-lineage-mcp": {
      "command": "/Users/xxxxx/xxxxx/Cline/MCP/quicksight-lineage-mcp/.venv/bin/python3",
      "args": [
        "/Users/xxxxx/xxxxx/Cline/MCP/quicksight-lineage-mcp/main.py"
      ],
      "disabled": false,
      "alwaysAllow": []
    }
}
}
```
4. Configue AWS CLi with AWS credentials have the necessary QuickSight permissions
5. Test connectivity with a simple resource listing command

## Common Use Cases

### Data Governance
- Track data lineage from source to dashboard
- Identify impact of data source changes
- Audit data dependencies across your organization

### Resource Management
- Inventory all QuickSight resources
- Identify unused or orphaned resources
- Plan resource migrations or cleanup

### Troubleshooting
- Diagnose dashboard or analysis issues by examining dependencies
- Understand data flow when reports show unexpected results
- Validate data source configurations

## Error Handling

The server provides detailed error messages for common issues:
- Invalid account IDs or resource IDs
- Insufficient permissions
- Resources not found
- Region-specific access issues

## Support

For issues or questions about this MCP server, please refer to the MCP documentation or contact your system administrator.

