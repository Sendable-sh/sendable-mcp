<p align="center">
  <a href="https://getsendable.io"><img src="logo.png" alt="Sendable" width="80" height="80"></a>
</p>

<h1 align="center">Sendable MCP server</h1>

<p align="center">
  Find LinkedIn prospects showing buying intent and run your outreach from Claude, ChatGPT, Cursor or any MCP client.
  <br>
  <a href="https://getsendable.io"><strong>Website</strong></a> ·
  <a href="https://docs.getsendable.io/integrations/mcp"><strong>Docs</strong></a> ·
  <a href="https://app.getsendable.io/signup"><strong>Sign up</strong></a>
</p>

---

[Sendable](https://getsendable.io) is a LinkedIn prospecting platform: signals find people showing buying intent, check them against your ideal customer, and feed campaigns that run your outreach on your own LinkedIn account.

This remote MCP server gives your AI assistant the same access you have in Sendable. Ask it in plain words:

- "Which campaigns got the most replies this week?"
- "Find the people who commented on this LinkedIn post and match our ideal customer."
- "Pause the signal on webinar attendees."
- "Draft a reply to every lead who asked for a demo."

## Server

| | |
| --- | --- |
| URL | `https://app.getsendable.io/api/mcp` |
| Transport | Streamable HTTP |
| Authentication | OAuth: sign in with your Sendable account, then pick a team. No API key. |
| Tools | 61 |

There is nothing to install or host: the server runs on Sendable.

## Connect your assistant

You need a [Sendable account](https://app.getsendable.io/signup). Each member connects their own assistant.

### Claude

In Sendable, open **Settings > Integrations > MCP** and click **Connect to Claude**. Or in Claude, open **Settings > Connectors**, click **Add custom connector** and paste the server URL.

### ChatGPT

Copy the server URL from **Settings > Integrations > MCP** in Sendable, then in ChatGPT open **Plugins**, click **Add**, then **Add custom MCP server** and paste the URL.

### Claude Code

```bash
claude mcp add --transport http sendable https://app.getsendable.io/api/mcp
```

Then run `/mcp` in Claude Code to sign in.

### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "sendable": {
      "url": "https://app.getsendable.io/api/mcp"
    }
  }
}
```

### VS Code

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "sendable": {
      "type": "http",
      "url": "https://app.getsendable.io/api/mcp"
    }
  }
}
```

### Other clients

Any client that supports remote MCP servers with OAuth works: paste the server URL in its custom connector setting.

When you connect, Sendable asks you to sign in, choose the team the assistant works on, and allow access. You can disconnect an assistant at any time from **Settings > Integrations > MCP**.

## What your assistant can do

The assistant works on one team at a time, with the same access as you. Launching a signal uses credits, and launching a campaign or sending a message acts on your LinkedIn accounts, exactly as in the app. Messages count toward each account's daily limits. To check a reply before it goes out, ask for a draft: it waits in the inbox, ready to send.

### Workspace

| Tool | What it does |
| --- | --- |
| `get_context` | Returns the team this connection works on, its credits, LinkedIn accounts and custom fields, and the other teams you can switch to. Call it first. |
| `switch_team` | Moves this connection to another of your Sendable teams. |

### Signals

| Tool | What it does |
| --- | --- |
| `list_signal_types` | Lists the sources a signal can watch, with what each one needs. |
| `get_ideal_customer_options` | Returns the accepted values of one ideal customer field. |
| `search_linkedin_locations` | Finds LinkedIn places (cities, regions) for the ideal customer's places field. |
| `list_signals` | Lists the team's signals with their status and lead counts. |
| `get_signal` | Returns one signal: status, source configuration, ideal customer, exclusions, campaign link, review counts, rejection memory and blacklist size. |
| `create_signal` | Creates a draft signal and optionally configures its source, ideal customer and exclusions. |
| `update_signal` | Changes a signal. |
| `duplicate_signal` | Copies a signal's configuration into a new draft. |
| `archive_signal` | Archives a signal (pausing it when live) or restores it from the archive. |
| `delete_signal` | Deletes a signal permanently with its collected profiles. |
| `launch_signal` | Launches a draft signal: it starts finding and qualifying people now, then every day. |
| `pause_signal` | Pauses a live signal, or cancels a resume still waiting for profiles in progress. |
| `resume_signal` | Resumes a paused signal. |
| `list_signal_leads` | Lists the people a signal found, newest first, with their qualification and approval. |
| `review_signal_leads` | Approves or rejects leads found by a signal. |
| `import_signal_csv_leads` | Adds people to a signal whose source type is CSV_IMPORT. |
| `remove_signal_csv_import` | Removes one imported file from a draft CSV signal, with the leads it brought. |
| `update_signal_blacklist` | Edits the people this signal must never keep, on a draft or paused signal. |
| `delete_signal_memory_rule` | Forgets one rejection reason the signal learned. |

### Campaigns

| Tool | What it does |
| --- | --- |
| `list_campaigns` | Lists the team's campaigns with their status and results. |
| `get_campaign` | Returns one campaign: its flow (nodes and edges), schedule, the signals feeding it, its progress and whether it can be launched. |
| `create_campaign` | Creates a draft campaign, optionally with its flow and schedule. |
| `import_campaign_file` | Downloads a file from a public URL into the workspace's files so a campaign can send it. |
| `update_campaign` | Changes a campaign's title, schedule or flow. |
| `duplicate_campaign` | Copies a campaign's flow and schedule into a new draft, without its leads. |
| `launch_campaign` | Launches or resumes a campaign: its LinkedIn accounts start acting on its leads within the schedule. |
| `pause_campaign` | Pauses a live campaign. |
| `archive_campaign` | Archives a campaign (pausing it when live) or restores it from the archive. |
| `delete_campaign` | Deletes a campaign that is not live, with its execution history. |
| `list_campaign_leads` | Lists the leads of a campaign with where they are in the flow (status, current step, why they stopped). |
| `add_campaign_leads` | Adds leads to a campaign: existing leads by leadId, and/or new people (at least a LinkedIn URL each). |
| `remove_campaign_leads` | Removes leads from a campaign and stops their pending actions. |
| `set_campaign_variant_winner` | Picks the winning variant of an A/B tested message step: every next lead receives it. |
| `link_signal_to_campaign` | Sends a signal's approved leads to a campaign, or unlinks it with campaignId null. |

### Leads

| Tool | What it does |
| --- | --- |
| `list_leads` | Lists the team's leads, newest first. |
| `get_lead` | Returns one lead with its custom fields, the signals that found it and its progress in each campaign. |
| `find_lead_contacts` | Looks up the email and/or phone number of leads with the team's own FullEnrich account, so it uses the team's FullEnrich credits. |
| `import_leads` | Adds people to the team's leads (at least a LinkedIn URL each), up to 500 per call. |
| `update_leads` | Changes fields, status or custom field values (by custom field key) of one or more leads. |
| `delete_leads` | Deletes leads permanently, everywhere: from every signal and campaign, with their history. |
| `get_team_blacklist` | Lists the people the team never contacts. |
| `update_team_blacklist` | Adds people to the team blacklist (LinkedIn URLs or existing leads), which stops every action on them, or removes entries. |
| `create_custom_field` | Creates a custom field on leads. |
| `update_custom_field` | Renames a custom field or edits the options of a SELECT field. |
| `delete_custom_field` | Deletes a custom field and its values on every lead. |

### Inbox

| Tool | What it does |
| --- | --- |
| `list_conversations` | Lists the team's LinkedIn conversations, latest message first. |
| `get_conversation` | Returns one conversation (contact, lead, label, draft, read and archive state) with a page of its latest messages. |
| `send_message` | Sends a text message from the conversation's LinkedIn account and clears its draft. |
| `edit_message` | Edits the text of a message the LinkedIn account sent. |
| `delete_message` | Deletes a message the LinkedIn account sent, for every participant. |
| `react_to_message` | Adds an emoji reaction to a message of the conversation. |
| `update_conversations` | Changes the state of one or more conversations: read or unread (synced to LinkedIn), archived, snoozed until a date, label, and Interested (marks the contact's lead Interested, creating the lead when needed). |
| `set_conversation_draft` | Writes the draft reply of a conversation, replacing any previous draft. |
| `list_labels` | Lists the team's inbox labels in sidebar order. |
| `create_label` | Creates an inbox label at the end of the sidebar. |
| `update_label` | Renames or recolors an inbox label. |
| `delete_label` | Deletes an inbox label; the conversations carrying it lose it. |
| `reorder_labels` | Reorders the inbox labels in the sidebar. |

### Analytics

| Tool | What it does |
| --- | --- |
| `get_team_analytics` | Reads the team's outreach results over a period: invitations sent and accepted, people contacted, replies and Interested leads, the same numbers as the Analytics dashboard. |

## Learn more

- [Sendable](https://getsendable.io): LinkedIn prospecting on buying signals
- [MCP documentation](https://docs.getsendable.io/integrations/mcp): setup, permissions and troubleshooting
- [Help center](https://docs.getsendable.io)
- [Sendable on LinkedIn](https://www.linkedin.com/company/sendable-outbound)
