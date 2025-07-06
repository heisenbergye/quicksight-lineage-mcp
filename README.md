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
```
#### 示例1
% q chat "AWS账号xxxxxxxxxx 在美东2的quicksight的dashboard数量"
✓ quicksight_lineage_mcp loaded in 1.20 s

🛠️  Using tool: list_dashboards from mcp server quicksight_lineage_mcp
 ⋮
 ● Running list_dashboards with the param:
 ⋮  {
 ⋮    "arguments": {
 ⋮      "account_id": "xxxxxxxxxx",
 ⋮      "region": "us-east-2"
 ⋮    },
 ⋮    "name": "list_dashboards"
 ⋮  }
Allow this action? Use 't' to trust (always allow) this tool for the session. [y/n/t]:

> t

> 在账户 xxxxxxxxxx 的美东2区域（us-east-2）中，QuickSight 共有 1个 dashboard：

• Dashboard ID: f0449be7-d2e6-4b9d-b405-3a93c08f7f05
• Dashboard 名称: url-test


#### 示例2

> 在账户 xxxxxxxxxx 的美西2区域的quicksight 所有的analysis和对应的dashboard以及使用的dataset的信息，如果analysis没有创建dashboard则忽略，结果以表格返回

> ## 账户 xxxxxxxxxx 美西2区域 QuickSight Analysis 资源表

| Analysis 名称 | Analysis ID | Dashboard 名称 | Dashboard ID | Dataset 名称 | Dataset ID | 可视化组件数量 |
|---------------|-------------|---------------|--------------|--------------|------------|---------------|
| covid analysis | 11beff09-7304-45f4-bddb-30db34ad1fc8 | geo | a9123e20-26d7-49ce-8758-027f4f5387c4 | covid | e4a6ac68-5849-47ed-88d3-97d829be9935 | 4 |

## 详细信息

Analysis "covid analysis" 包含：
• **对应 Dashboard**: "geo"
• **使用的 Dataset**: "covid"
• **可视化组件**: 4个
  1. **GeospatialMapVisual** - 地理空间地图（使用经纬度和新增病例数据）
  2. **FilledMapVisual** - 填充地图（按国家代码显示新增病例）
  3. **PluginVisual** - Highcharts 插件可视化（热力图样式的KPI展示）
  4. **LayerMapVisual** - 图层地图（多边形图层）

## 总结

在账户 xxxxxxxxxx 的美西2区域中：
• **Analysis 总数**: 2个
• **已创建 Dashboard 的 Analysis**: 1个（covid analysis）
• **未创建 Dashboard 的 Analysis**: 1个（slab01_cur analysis，已忽略）

注意：根据您的要求，"slab01_cur analysis" 由于没有对应的 dashboard，已从结果中忽略。


> 获取 dataset e4a6ac68-5849-47ed-88d3-97d829be9935 对应的datasource的信息

> ## 账户 xxxxxxxxxx 美西2区域 Dataset e4a6ac68-5849-47ed-88d3-97d829be9935 数据源信息表

| 数据源名称 | 数据源 ID | 数据源类型 | 工作组 | 数据库 | 表名 | 连接方式 | 字段信息 |
|-----------|-----------|-----------|--------|--------|------|----------|----------|
| geodemo | 692fcea1-5d2e-4624-96d4-092f9489d738 | Amazon Athena | primary | AwsDataCatalog.default | covid | SPICE | 主要COVID数据表 |
| country | 87ce7efb-6db8-47b4-8dc2-18597322a7ef | Amazon Athena | primary | AwsDataCatalog.default | country | DIRECT_QUERY | 国家地理信息表 |

## 详细数据源配置

### 主数据源 - geodemo (COVID数据)
• **数据源类型**: Amazon Athena
• **工作组**: primary
• **数据库**: AwsDataCatalog
• **Schema**: default
• **表名**: covid
• **导入模式**: SPICE (内存中缓存)
• **字段结构**:
  • date_reported (STRING) - 报告日期
  • country_code (STRING) - 国家代码
  • country (STRING) - 国家名称
  • who_region (STRING) - WHO区域
  • new_cases (INTEGER) - 新增病例
  • cumulative_cases (INTEGER) - 累计病例
  • new_deaths (INTEGER) - 新增死亡
  • cumulative_deaths (INTEGER) - 累计死亡

### 关联数据源 - country (国家信息)
• **数据源类型**: Amazon Athena
• **工作组**: primary
• **数据库**: AwsDataCatalog
• **Schema**: default
• **表名**: country
• **导入模式**: DIRECT_QUERY (实时查询)
• **字段结构**:
  • country (STRING) - 国家名称
  • alpha2 (STRING) - ISO 2位国家代码
  • alpha3 (STRING) - ISO 3位国家代码
  • numeric (INTEGER) - 数字国家代码
  • latitude (DECIMAL) - 纬度
  • longitude (DECIMAL) - 经度

## 数据关联关系

该 dataset 通过 LEFT JOIN 将两个数据源关联：
• **连接条件**: {country_code} = alpha2
• **连接类型**: LEFT JOIN
• **目的**: 将COVID数据与国家地理信息结合，支持地理可视化

## 地理标记配置

数据集中包含地理角色标记：
• country_code 和 country 标记为 COUNTRY 角色
• who_region 标记为 STATE 角色
• latitude 标记为 LATITUDE 角色
• longitude 标记为 LONGITUDE 角色

这些标记使得 QuickSight 能够自动识别地理数据并支持地图可视化功能。


#### 示例3

> 基于分析结果，我可以看到 "hotel_booking analysis" 使用了两个数据集，但由于数据集类型不支持通过 API 获取详细信息，我将基于可用信息提供表格。从数据源列表中可以看到有多个与 hotel_booking 相关的 CSV 文件数
据源。

## 账户 xxxxxxxxxx 美东1区域 QuickSight Analysis "hotel_booking analysis" 数据源信息表

| Dataset 名称 | Dataset ID | 数据源类型 | 可能的数据源名称 | 数据源 ID | 备注 |
|-------------|------------|-----------|----------------|-----------|------|
| hotel_booking | cc805b4c-9caa-440a-86b2-cf183bd82eed | 文件上传 | hotel_bookings.csv 或 hotel_booking.csv | 19e54350-8aff-45a1-b387-de4471681639<br/>684c12e1-f270-42af-83e4-31e513910e12<br/>
8141046c-5040-4856-9e56-c62eef14c800<br/>9b07ac49-40a5-4004-9676-03e83c4565b9 | 主要数据集，包含酒店预订数据 |
| img_url_test.xlsx | 9aa06a27-e524-43a0-a514-0e5d27b279dc | 文件上传 | img_url_test.xlsx | f358ad27-3042-4153-85ec-3828add5c87d<br/>fe09f92c-b2c9-4c6c-8767-ea76154df336 | 辅助数据集，用于图片URL显示 |

## 数据源详细信息

### 主要数据源特征：
1. hotel_booking 数据集：
   • 包含酒店预订相关字段：is_canceled, hotel, reservation_status_date, adr, market_segment, country, customer_type 等
   • 支持取消率分析、收入分析、地理分析等多维度分析
   • 数据类型：CSV 文件上传

2. img_url_test.xlsx 数据集：
   • 包含图片URL字段，用于在分析中显示图像
   • 主要用于表格可视化中的图片展示功能
   • 数据类型：Excel 文件上传

## 限制说明

由于这些数据集的类型不支持通过 QuickSight API 获取详细的数据源配置信息，无法提供更具体的数据源连接参数、表结构等详细信息。这通常发生在：
• 直接文件上传的数据集
• 某些特定类型的数据连接

```

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

