# Cline Setup

Career-Ops supports Cline through the `.clinerules` file and the `career-ops` skill.

If your Cline client reads project instructions automatically, `.clinerules`
is enough for routing and behavior. Cline should reuse the same checked-in
mode files, templates, tracker flow, and scripts that already power the
Claude workflow.

## Prerequisites

- Cline extension installed in VS Code
- Node.js 18+
- Playwright Chromium installed for PDF generation and reliable job verification
- Go 1.21+ if you want the TUI dashboard

## Install

```bash
npm install
npx playwright install chromium
```

## Using the career-ops Skill

Cline has access to the `career-ops` skill which provides a command center for all job search operations. To use it:

1. Type or say: "use the career-ops skill" or just mention career-ops
2. The skill will be activated and you can use any of the available commands

## Available Commands

| Command | Description |
|---------|-------------|
| (no args) | Show command menu |
| JD text or URL | Auto-pipeline: evaluate + report + PDF + tracker |
| `pipeline` | Process pending URLs from inbox |
| `oferta` | Evaluation only (A-F scoring) |
| `ofertas` | Compare and rank multiple offers |
| `contacto` | LinkedIn outreach: find contacts + draft message |
| `deep` | Deep company research |
| `pdf` | Generate ATS-optimized CV PDF |
| `training` | Evaluate course/cert against goals |
| `project` | Evaluate portfolio project idea |
| `tracker` | Application status overview |
| `apply` | Live application assistant |
| `scan` | Scan portals for new offers |
| `batch` | Batch processing with parallel workers |
| `patterns` | Analyze rejection patterns |
| `followup` | Follow-up cadence tracker |

## Recommended Starting Prompts

- `Use the career-ops skill and evaluate this job URL: [URL]`
- `Use career-ops to scan my configured portals for new roles that match my profile`
- `Generate a tailored ATS PDF for this role using career-ops`
- `Run the career-ops pipeline to process pending URLs`

## Routing Map

| User intent | Files Cline should read |
|-------------|-------------------------|
| Raw JD text or job URL | `modes/_shared.md` + `modes/auto-pipeline.md` |
| Single evaluation only | `modes/_shared.md` + `modes/oferta.md` |
| Multiple offers | `modes/_shared.md` + `modes/ofertas.md` |
| Portal scan | `modes/_shared.md` + `modes/scan.md` |
| PDF generation | `modes/_shared.md` + `modes/pdf.md` |
| Live application help | `modes/_shared.md` + `modes/apply.md` |
| Pipeline inbox processing | `modes/_shared.md` + `modes/pipeline.md` |
| Tracker status | `modes/tracker.md` |
| Deep company research | `modes/deep.md` |
| Training / certification review | `modes/training.md` |
| Project evaluation | `modes/project.md` |

The key point: Cline support is additive. It should route into the existing
Career-Ops modes and scripts rather than introducing a parallel automation
layer.

## Behavioral Rules

- Treat raw JD text or a job URL as the full auto-pipeline path unless the user explicitly asks for evaluation only.
- Keep all personalization in `config/profile.yml`, `modes/_profile.md`, `article-digest.md`, or `portals.yml`.
- Never verify a job's live status with generic web fetch when Playwright is available.
- Never submit an application for the user.
- Never add new tracker rows directly to `data/applications.md`; use the TSV addition flow and `merge-tracker.mjs`.

## First Run Setup

On first use, Cline will check for required files and guide you through setup:

1. **CV** (`cv.md`) - Your CV in markdown format
2. **Profile** (`config/profile.yml`) - Your personal details and preferences
3. **Profile customization** (`modes/_profile.md`) - Archetypes and targeting
4. **Portals** (`portals.yml`) - Job portal configuration

If any files are missing, Cline will help you create them.

## LinkedIn Job Support

Career-Ops supports LinkedIn job postings in multiple ways:

### Pasting LinkedIn Job URLs

When you paste a LinkedIn job URL (e.g., `https://www.linkedin.com/jobs/view/123456789`), Career-Ops will:

1. **Attempt Playwright extraction** - Navigate to the job page and extract details
2. **Try the guest API** - If login is required, use the public job API endpoint
3. **Request manual paste** - If both fail, ask you to paste the JD text

### LinkedIn Portal Scanning

The `portals.yml` configuration includes LinkedIn search queries that discover jobs via WebSearch:

```yaml
- name: LinkedIn — AI Engineer
  query: 'site:linkedin.com/jobs "AI Engineer" OR "ML Engineer" remote'
  enabled: true
```

### Supported LinkedIn URL Formats

- `https://www.linkedin.com/jobs/view/{job-id}` (standard)
- `https://linkedin.com/jobs/view/{job-id}` (short)
- `https://www.linkedin.com/jobs/collections/recommended/?currentJobId={job-id}` (from collections)

### Limitations

- LinkedIn may require login for some job postings
- Guest API has rate limits
- Results may vary by geographic location
- Maximum ~1000 results per search

## Verification

```bash
npm run verify

# optional dashboard build
cd dashboard && go build ./...
```

## Differences from Claude Code

Cline works similarly to Claude Code with a few differences:

1. **Skill invocation**: Use `use_skill` with the skill name "career-ops"
2. **No subagent delegation**: Cline handles all operations directly without subagents
3. **Same mode files**: All mode files in `modes/` are shared between Claude Code and Cline

## Troubleshooting

### Skill not found
Make sure the `.clinerules` file exists in the project root and contains the skill definition.

### Playwright errors
Run `npx playwright install chromium` to ensure the browser is installed.

### Missing profile
Copy `config/profile.example.yml` to `config/profile.yml` and fill in your details.