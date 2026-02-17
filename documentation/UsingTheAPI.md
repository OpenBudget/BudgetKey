# Using the BudgetKey API

The BudgetKey API server (`https://next.obudget.org`) exposes several APIs for querying Israeli government budget, procurement, and spending data.

## Table of Contents

- [Query API](#query-api) — SQL queries against structured database tables
- [Search API](#search-api) — Full-text search across all document types
- [SimpleDB API](#simpledb-api) — Simplified table info, search, and query
- [Lists API](#lists-api) — Save and manage personal lists of items (requires authentication)
- [Auth API](#auth-api) — Authentication via Google/GitHub OAuth

---

## Query API

**Base URL:** `https://next.obudget.org/api/`

The Query API wraps the [apisql](https://github.com/dataspot/apisql) library and provides read-only SQL access to the BudgetKey PostgreSQL database.

### Endpoints

#### `GET /api/query`

Run a SQL query against the database.

**Query Parameters:**

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `query`   | string | Yes      | A SQL SELECT statement |
| `page`    | int    | No       | Page number for pagination (default: 0) |
| `page_size`| int   | No       | Rows per page (default/max: 1000) |

**Response:**

```json
{
  "rows": [
    { "column1": "value1", "column2": 123 }
  ],
  "total": 4200,
  "page_size": 1000,
  "download_url": "/api/download?..."
}
```

**Caching:** Responses are cached for 1 hour (3600 seconds).

**Example:**

```
GET https://next.obudget.org/api/query?query=SELECT code, title, net_revised FROM budget_items_data WHERE year=2023 LIMIT 10
```

#### `GET /api/download`

Download full query results (all pages) as a file. The URL is returned in the `download_url` field of a `/api/query` response.

---

## Search API

**Base URL:** `https://next.obudget.org/`

The Search API wraps the [apies](https://github.com/OpenBudget/apies) library and provides full-text search over an Elasticsearch index.

### Endpoints

#### `GET /search/<doc-type>`

Search within a specific document type, or across all types using `all`.

**Supported `<doc-type>` values:**

| doc-type | Description |
|---|---|
| `budget` | National budget line items |
| `national-budget-changes` | Budget change requests and amendments |
| `contract-spending` | Government contract spending |
| `supports` | Government grants and subsidies to organisations |
| `tenders` | Procurement tenders |
| `entities` | Legal entities (companies, non-profits, etc.) |
| `people` | Public officials and their roles |
| `reports` | Published government reports |
| `gov_decisions` | Government decisions |
| `calls_for_bids` | Calls for bids / procurement notices |
| `support_criteria` | Criteria for support programs |
| `activities` | Government activities |
| `muni_budgets` | Municipal budgets |
| `muni_tenders` | Municipal tenders |
| `support_programs` | Government support programs |
| `all` | Search across all document types simultaneously |

**Query Parameters:**

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `q`       | string | No       | Free-text search query |
| `size`    | int    | No       | Number of results to return (default: 10) |
| `from`    | int    | No       | Offset for pagination (default: 0) |
| `filters` | JSON   | No       | Filter conditions (JSON array of filter objects) |
| `highlight`| JSON  | No       | Fields to highlight in results |
| `score_threshold` | float | No | Minimum relevance score threshold |

**Response:**

```json
{
  "search_results": [
    {
      "source": { ... },
      "highlight": { ... },
      "score": 12.34
    }
  ],
  "search_counts": {
    "_current": {
      "total_overall": 4200
    },
    "budget": { "total_overall": 1234 },
    "tenders": { "total_overall": 56 }
  }
}
```

**Caching:** Responses are cached for 10 minutes (600 seconds).

**Semantic search:** When an OpenAI API key is configured, search terms of 5 or more characters also trigger vector embedding search using `text-embedding-3-small`, boosted at 0.2 relative to text match.

**Example:**

```
GET https://next.obudget.org/search/budget?q=education&size=5
GET https://next.obudget.org/search/all?q=defense+ministry
```

#### `GET /get/<doc-type>/<doc-id>`

Retrieve a single document by its ID.

**Response:** The document's full source object.

#### `GET /doc-counts`

Return record counts across all indexed document types.

---

## SimpleDB API

**Base URL:** `https://next.obudget.org/api/`

The SimpleDB API provides a higher-level, table-oriented interface for querying structured data without needing to know SQL. It also exposes table schema information fetched from canonical datapackage descriptors.

### Available Tables

| Table Name | Description |
|---|---|
| `budget_items_data` | National budget line items |
| `income_items_data` | National income items |
| `support_programs_data` | Government support programs |
| `supports_transactions_data` | Support payment transactions |
| `contracts_data` | Government contracts |
| `entities_data` | Legal entities |
| `budgetary_change_requests_data` | Budget change requests |
| `budgetary_change_transactions_data` | Budget change transactions |

### Endpoints

#### `GET /api/tables`

List all available tables.

**Response:**

```json
["budget_items_data", "income_items_data", "supports_transactions_data", ...]
```

---

#### `GET /api/tables/<table>/info`

Retrieve schema and metadata for a specific table.

**Response:**

```json
{
  "description": "Human-readable description of this table",
  "fields": [
    {
      "name": "field_name",
      "title": "Human-readable field title",
      "description": "Field description",
      "type": "string|number|integer|date|boolean|array|object"
    }
  ],
  "schema": "CREATE TABLE budget_items_data (...)"
}
```

Returns `404` if the table does not exist.

---

#### `GET /api/tables/<table>/query`

Run a SQL query scoped to a specific table.

**Query Parameters:**

| Parameter   | Type   | Required | Description |
|-------------|--------|----------|-------------|
| `query`     | string | Yes      | SQL SELECT statement |
| `page_size` | int    | No       | Max rows to return (default: 100) |

**Response:**

```json
{
  "rows": [ { ... } ],
  "num_rows": 42,
  "total_rows": 1234,
  "download_url": "https://next.obudget.org/api/download?...",
  "warnings": ["Optional warnings about the query"]
}
```

The response is capped at 120 KB. If results exceed this, rows are truncated and `num_rows` reflects the actual returned count.

**Common query warnings:**

- Using `code LIKE '12.34%'` in `budget_items_data` (counts items multiple times across budget levels)
- Mixing budget codes of different levels in the same query
- Using `GROUP BY item_url`

**Example:**

```
GET https://next.obudget.org/api/tables/budget_items_data/query?query=SELECT code, title, net_revised FROM budget_items_data WHERE year=2023 AND level=3 LIMIT 20
```

---

#### `GET /api/tables/<table>/search`

Free-text search over a table's records via Elasticsearch (where available).

**Query Parameters:**

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `q`       | string | No       | Search query |

**Response:**

```json
{
  "search_results": [
    {
      "field1": "value1",
      "field2": 123
    }
  ],
  "num_results": 20,
  "total_results": 4200
}
```

Returns an error object if search is not configured for that table:
```json
{ "error": "Search not available for table \"...\", use DB query instead" }
```

The response is capped at 120 KB.

**Caching:** Responses are cached for 10 minutes (600 seconds).

---

## Lists API

**Base URL:** `https://next.obudget.org/lists/`

The Lists API allows authenticated users to save, organise, and retrieve lists of BudgetKey items (e.g. saved searches, bookmarks).

### Authentication

All write operations and reads of private lists require an `auth-token` JWT header:

```
auth-token: <JWT token>
```

Tokens are obtained via the [Auth API](#auth-api).

**Cache:** All list responses have `Cache-Control: no-cache`.

### Data Model

A **list** has:

| Field        | Type    | Description |
|--------------|---------|-------------|
| `id`         | int     | Auto-generated list ID |
| `name`       | string  | List name (unique per user); auto-generated UUID if omitted |
| `user_id`    | string  | Owner's user ID |
| `title`      | string  | Human-readable list title |
| `kind`       | string  | Category/type tag for the list |
| `visibility` | int     | `0` = private, `1`+ = visible to others (≥`2` publicly accessible by user ID) |
| `properties` | object  | Arbitrary JSON metadata |
| `create_time`| datetime| ISO 8601 |
| `update_time`| datetime| ISO 8601 |

An **item** in a list has:

| Field        | Type    | Description |
|--------------|---------|-------------|
| `id`         | int     | Auto-generated item ID |
| `list_id`    | int     | ID of the parent list |
| `url`        | string  | URL identifying the item (required) |
| `title`      | string  | Human-readable title (required) |
| `properties` | object  | Arbitrary JSON metadata |
| `create_time`| datetime| ISO 8601 |
| `update_time`| datetime| ISO 8601 |

### Endpoints

#### `GET /lists/`

Read list(s) or item(s) for the authenticated user.

**Query Parameters:**

| Parameter | Type    | Description |
|-----------|---------|-------------|
| `list`    | string  | List name to fetch (omit to fetch all lists) |
| `items`   | boolean | If truthy, include items in the response |
| `kind`    | string  | Filter lists/items by kind |
| `user_id` | string  | Fetch another user's lists (only works for lists with `visibility ≥ 2`) |

**Response — single list (with items):**
```json
{
  "id": 1,
  "name": "saved-searches",
  "user_id": "user-id-123",
  "title": "My Saved Searches",
  "kind": "searches",
  "visibility": 0,
  "properties": {},
  "create_time": "2024-01-01T00:00:00",
  "update_time": "2024-01-01T00:00:00",
  "items": [
    {
      "id": 10,
      "list_id": 1,
      "url": "https://next.obudget.org/s?q=education",
      "title": "Education spending",
      "properties": { "filters": {} },
      "create_time": "2024-01-01T00:00:00",
      "update_time": "2024-01-01T00:00:00"
    }
  ]
}
```

**Response — all lists:**
```json
[
  { "id": 1, "name": "saved-searches", "user_id": "...", ... },
  { "id": 2, "name": "bookmarks", "user_id": "...", ... }
]
```

**Response — all items across lists:**
```json
[
  { "id": 10, "list_id": 1, "url": "...", "title": "...", "properties": {}, ... }
]
```

**Error responses:**
```json
{ "success": false, "name": "list-name", "user_id": "user-id" }
{ "success": false, "error": "permission denied" }
```

---

#### `PUT /lists/`

Add an item to a list, or update list metadata.

**Query Parameters:**

| Parameter | Type    | Description |
|-----------|---------|-------------|
| `list`    | string  | List name (auto-created if it doesn't exist; omit to get a UUID-named list) |
| `self`    | boolean | If truthy, update the list's own metadata instead of adding an item |

**Request body (add item):**
```json
{
  "url": "https://next.obudget.org/i/budget/...",
  "title": "Item title",
  "properties": { "any": "extra data" }
}
```

`url` and `title` are required. If an item with the same `url` and `title` already exists in the list, its `properties` are updated rather than creating a duplicate.

**Request body (update list metadata, `self=true`):**
```json
{
  "title": "My List Title",
  "kind": "bookmarks",
  "visibility": 1,
  "properties": { "icon": "star" }
}
```

**Response (added/updated item):**
```json
{
  "id": 10,
  "list_id": 1,
  "list_name": "saved-searches",
  "url": "https://...",
  "title": "Item title",
  "properties": { "any": "extra data" },
  "create_time": "2024-01-01T00:00:00",
  "update_time": "2024-01-01T00:00:00"
}
```

---

#### `DELETE /lists/`

Delete an item or all items in a list.

**Query Parameters:**

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `list`    | string | Yes      | List name |
| `item_id` | string | Yes      | Item ID to delete, or `"all"` to delete the entire list and all its items |

**Response (success):**
```json
{ "success": true }
```

**Response (failure):**
```json
{ "success": false }
{ "success": false, "error": "missing required parameter" }
{ "success": false, "error": "permission denied" }
```

---

## Auth API

**Base URL:** `https://next.obudget.org/auth/`

Authentication is handled via OAuth2 (Google and GitHub) using the [dgp-oauth2](https://github.com/dataspot/dgp-oauth2) library.

### Endpoints

The auth endpoints follow the standard dgp-oauth2 flow:

| Endpoint | Description |
|---|---|
| `GET /auth/authorize` | Begin OAuth flow, redirect to provider |
| `GET /auth/callback` | OAuth callback, issues a JWT session token |
| `GET /auth/me` | Returns the current user's profile and JWT token |
| `GET /auth/logout` | Logs out the current user |

The JWT token returned from `/auth/me` is passed as the `auth-token` header for Lists API requests.

---

## Data Types Reference

### Budget (`budget`)

National budget line items by year, code, and classification.

**Key fields:** `code`, `title`, `year`, `depth`, `net_allocated`, `net_revised`, `net_executed`, `hierarchy`, `func_cls_title_1`, `econ_cls_title_1`

### Tenders (`tenders`)

Procurement tender notices.

**Key fields:** `publication_id`, `tender_id`, `tender_type`, `description`, `publisher`, `status`, `publication_date`, `volume`, `supplier`, `decision`, `documents`, `awardees`

### Contract Spending (`contract-spending`)

Government contract payments, with links to tenders and budget codes.

**Key fields:** `order_id`, `purpose`, `publisher`, `supplier_name`, `supplier_code`, `volume`, `executed`, `order_date`, `end_date`, `budget_code`, `payments`, `tender_key`

### Supports (`supports`)

Government grants and subsidies to organisations, grouped by budget code and recipient.

**Key fields:** `budget_code`, `year_requested`, `recipient`, `entity_id`, `request_type`, `amount_total`, `payments`

### National Budget Changes (`national-budget-changes`)

Budget amendment requests to the Knesset Finance Committee.

**Key fields:** `transaction_id`, `year`, `req_title`, `change_type_name`, `amount`, `net_expense_diff`, `explanation`, `is_pending`, `date`

### Entities (`entities`)

Legal entities (companies, NGOs, government bodies, etc.).

**Key fields:** `id`, `name`, `kind`, `entity_id`, `website`, `address`, `related_entities`, `contracts_count`, `supports_count`

### People (`people`)

Public officials and appointment records.

**Key fields:** `key`, `person_key`, `details` (array of roles with `full_name`, `position`, `company`, `when`, `source`)

### Municipal Budgets (`muni_budgets`)

Local authority budget data.

### Municipal Tenders (`muni_tenders`)

Local authority procurement tenders.

### Government Decisions (`gov_decisions`)

Cabinet/government committee decisions.

### Reports (`reports`)

Published government accountability and audit reports.

### Calls for Bids (`calls_for_bids`)

Open calls for bids / expressions of interest.

### Support Criteria (`support_criteria`)

Official criteria and conditions for government support programs.

### Support Programs (`support_programs`)

Structured records of government support programs.

### Activities (`activities`)

Government program activities.

---

## Notes

- All API responses are JSON.
- CORS is enabled for all endpoints.
- The server runs behind `gunicorn` on port 5000, typically proxied at `https://next.obudget.org`.
- All monetary values are in Israeli New Shekel (ILS) unless otherwise noted.
- Budget codes follow a hierarchical dotted notation (e.g. `20.67.01.13`), where `depth`/`level` indicates the hierarchy level.
