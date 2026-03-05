---
name: myaider-skill-importer
description: Import and create skills from MyAider MCP. Use this skill whenever the user wants to import their MyAider MCP skills into agent skills. This skill checks if MyAider MCP is configured, retrieves available skills, presents them to the user for selection, and uses skill-creator to create each selected skill properly.
compatibility: []
---

# MyAider Skill Importer

## Purpose
Automate the process of importing skills from the MyAider MCP server into agent skills. This skill retrieves available skills, lets the user choose which ones to import, and creates proper skill files for each using existing skill-creator skill.

## MANDATORY WORKFLOW

### Step 0 — REQUIRED: Discover MyAider MCP Server Name and Check skill-creator Skill

**Note on naming convention:** MCP tool identifiers follow the format `mcp__<server-name>__<tool-name>`. The server name is whatever the user chose when configuring the MCP — it may not be `myaider`. Always discover the actual name rather than assuming it.

#### Phase A — Discover the server name via tool search
Use `tool_search_tool_regex` with the pattern `getSkills` to find any MCP tool whose name or description mentions `getSkills`. This will return matching tool identifiers from which you can extract the server name (the middle segment of `mcp__<server-name>__getSkills`).

Store the discovered server name as `{SERVER_NAME}`. Use `mcp__{SERVER_NAME}__getSkills` for all subsequent calls.

#### Phase B — Branch on the result

- **Exactly 1 match found** → Extract the server name from the tool identifier, proceed silently to Step 1.
- **0 matches found** → Inform the user that MyAider MCP needs to be set up first:

  > The MyAider MCP server doesn't appear to be configured. To use this skill, you need to set up the MyAider MCP server first.
  >
  > **Setup Instructions:**
  > 1. Go to https://www.myaider.ai/mcp
  > 2. Follow the instructions to configure the MyAider MCP server for your agent
  > 3. Once configured, come back and ask me to import your MyAider skills

  Do NOT proceed until the user confirms MyAider is configured.

- **2 or more matches found** → Multiple MCP servers expose a `getSkills` tool. List all discovered server names and ask the user to confirm which one is their MyAider instance. Use the user's answer as `{SERVER_NAME}`.

Check if skill-creator skill is available; if not, ask the user to install skill-creator.

### Step 1 — REQUIRED: Get Available Skills
Call `mcp__{SERVER_NAME}__getSkills` (using the server name discovered in Step 0) with an empty object `{}` to retrieve all available skills from MyAider.

### Step 2 — REQUIRED: Present Skills to User
Present the list of skills to the user with their descriptions. Ask them to choose:
- "All" - import every skill
- Or specify which specific skills they want (by name)

Wait for user confirmation before proceeding.

### Step 3 — REQUIRED: For Each Selected Skill
For each skill the user wants to import:

1. **Extract the skill specification** from the getSkills result:
   - Skill name
   - Description (from the Usage Instructions or summary)
   - Usage Instructions (the main content)
   - **Tools with FULL usage details**: Extract each tool's name, description, and parameter schema from the "Tools" section in the getSkills result

2. **Create a properly formatted skill using skill-creator**:
   YOU MUST create the skill automatically instead of ask user to do it manually. YOU MUST use the Skill tool to invoke `skill-creator:skill-creator` with this template:

   ```
   Create a new skill called "[skill-name]" based on the following specification:

   ## Skill Name
   [skill-name]

   ## Description
   [description - make it comprehensive with triggering guidance]

   ## Usage Instructions
   [full usage instructions from the myaider skill]

   ## Tools (MCP {SERVER_NAME})
   This skill uses the following MCP tools from {SERVER_NAME}. Include the full tool descriptions and parameter schemas BELOW to optimize token usage - the skill should NOT rely on the MCP protocol to get tool descriptions:

   ### [tool-name-1]
   [full tool description from getSkills result]

   **Parameters:**
   [parameter schema - include all parameters with their types, required/optional status, and descriptions]

   ### [tool-name-2]
   [full tool description from getSkills result]

   **Parameters:**
   [parameter schema - include all parameters with their types, required/optional status, and descriptions]
   ```

   **Critical**: The extracted tool descriptions and schemas must be included directly in the skill to avoid MCP protocol overhead. This optimizes token usage by enabling the skill to function without calling the MCP protocol for tool introspection.

3. **Confirm creation** to the user after each skill is created

### Step 4 — REQUIRED: Summarize
After all selected skills are created, provide a summary:
- List of successfully created skills
- File locations
- Any skills that failed (if any)

## Important Constraints
- Always discover the MCP server name via tool search first (Step 0) — do NOT hardcode `myaider` or any other name
- Always call `getSkills` after confirming MCP is configured — do NOT guess what skills are available
- **Always extract and include FULL tool descriptions and schemas** from getSkills — this is critical to optimize token usage. The created skill should work without needing MCP protocol tool introspection
- Use the discovered `{SERVER_NAME}` consistently for all MCP tool calls and in generated skill files
- Always wait for user selection before creating skills
- Create skills one at a time using skill-creator
- Keep the skill-creator conversation focused on each skill creation

## Example Usage
- "Import my MyAider skills"
- "Create skills from myaider"
- "Set up the skills from my MyAider MCP"
