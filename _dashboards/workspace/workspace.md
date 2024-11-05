---
layout: default
title: Workspace for OpenSearch Dashboards
nav_order: 110
has_children: true
---

# Workspace for OpenSearch Dashboards
Introduced 2.18
{: .label .label-purple }

The Workspace feature in OpenSearch Dashboards enables you to tailor your environment with use case-specific configurations. For example, you can create dedicated workspaces for observability scenarios, allowing you to focus on relevant functionalities. Additionally, the Workspace feature enables organization of visual assets such as dashboards and visualizations within a workspace that isolated storage. This capability enhances workflow precision and flexibility for customizing data exploration and analysis.

## Workspace data model

The Workspace data model is defined by the following structure: 

```typescript
interface Workspace {
  id: string;
  name: string;
  description?: string;
  features?: string[];
  color: string;
  uiSettings: Record<string, unknown>;
}
```
{% include copy-curl.html %}

The Workspace data model is composed of the following key attributes:

- `id`: String type; unique identifier for each each workspace.
- `name`: String type; designates the workspace's name.
- `description`: Optional string type; provides contextual information for the workspace.
- `features`: Optional array of strings; contains use case IDs linked to the workspace.

---

#### Example Workspace object

The following object shows a typical Workspace configuration:

```typescript
{
  id: "M5NqCu",
  name: "Analytics team",
  description: "Analytics team workspace",
  features: ["use-case-analytics"],
}
```
{% include copy-curl.html %}

The configuration creates an analytics team created with the `use-case-observability` feature set. Use cases map to specific OpenSearch Dashboards feature groups, limiting functionality to the defined set within each workspace. 

Predefined use case options:

- `use-case-observability`
- `use-case-security-analytics`
- `use-case-search`
- `use-case-essentials`
- `use-case-all`

---

## Associating saved objects with workspaces

Saved objects in OpenSearch Dashboards, such as dashboards, visualizations, and index patterns, can be associated with specifif workspaces, improviing organization and accessibility as the volume of objects grows.

The `workspaces` attribute, an array of strings, is added to saved objects to be linked with one or more workspaces. As a result, saved objects such as dashboards and visualizations are only accessible within their designated workspaces. 

The following saved object shows a dashboard object associated with the workspace by `M5NqCu`:

```typescript
{
  type: "dashboard",
  id: "da123f20-6680-11ee-93fa-df944ec23359",
  workspaces: ["M5NqCu"]
}
```
{% include copy-curl.html %}

Saved objects support association with multiple workspaces, facilitating cross-team collaboration and resource sharing. This feature is useful when an object is relevant to various team, projects, or use cases. 

The following example shows a data source object linked to multiple workspaces:

```typescript
{
  type: "data-source",
  id: "da123f20-6680-11ee-93fa-df944ec23359",
  workspaces: ["M5NqCu", "<TeamA-workspace-id>", "<Analytics-workspace-id>"]
}
```
{% include copy-curl.html %}

## Non-workspace saved objects

Not all saved objects in OpenSearch Dashboards are workspace-associated. Some objects operate independently of workspace framework. These objects lack `workspace` attribute and serve system-wide functions. For example, the global user interface settings object manages configurations affecting the entire OpenSearch Dashboards interface to maintain consistent functionality across all workspaces.

This dual approach allows OpenSearch Dashboards to balance granular, context-specific customization with overall system consistency. 

## Enabling the Workspace feature

In your `opensearch_dashboards.yml` file, set the following option:

```yaml
workspace.enabled: true
uiSettings:
  overrides:
    "home:useNewHomePage": true
```
{% include copy-curl.html %}

If your cluster has the Security plugin installed, then multi-tenancy must be disabled to avoid conflicts with workspaces providing similar functionality:

```yaml
opensearch_security.multitenancy.enabled: false
```
{% include copy-curl.html %}
