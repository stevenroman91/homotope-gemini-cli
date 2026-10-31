# Homotope for Gemini CLI

Vectorize images from [Gemini CLI](https://github.com/google-gemini/gemini-cli) with
[Homotope](https://homotope.app/en/): PNG or JPEG in, SVG, PDF, EPS or DXF out, plus a
cut-readiness check of the DXF before it goes to a laser, a vinyl cutter or a plotter.

This extension adds Homotope's remote MCP server (`https://homotope.app/mcp`) to Gemini CLI. It
contains no code and no key: only the server address and short instructions for the model
(`GEMINI.md`).

## Install

```bash
gemini extensions install https://github.com/stevenroman91/homotope-gemini-cli
```

Then start Gemini CLI. The first time a Homotope tool is used, Gemini CLI opens Homotope's
sign-in page in your browser (OAuth 2.1): sign in with your Google account and authorise the
access. If the browser does not open, run `/mcp auth homotope` in Gemini CLI.

To remove the access later: Homotope, “My account” page, “Connected applications” section,
“Revoke”.

## What it costs

Through an assistant, every job is a server job: the image is sent to Homotope, and the job
counts against the free plan’s 2 trial jobs of the month (the same ones as in the web app), then
the Pro quota, then credits. In the browser, in
the [Homotope workshop](https://homotope.app/atelier), nothing is sent. Plans and prices:
[homotope.app/en/pricing.html](https://homotope.app/en/pricing.html).

## Tools

| Tool                  | What it does                                                         |
| --------------------- | -------------------------------------------------------------------- |
| `vectorize_image`     | Sends a PNG or JPEG image (base64 or public https URL), starts a job |
| `get_job`             | Job status: queued, running, done, failed, cancelled, interrupted    |
| `get_svg`             | The SVG of a finished job                                            |
| `get_result`          | The result as SVG, PDF, EPS or DXF, colour separations included      |
|                       | (or the sticker cut contour alone, as a DXF)                         |
| `check_cut`           | “Ready to cut?” report of the DXF                                    |
| `list_vectorizations` | The account's latest jobs                                            |
| `suggest_preset`      | The preset suggested for an image, with its reasons (free, no job)   |
| `get_preview`         | PNG preview of a finished job (result, source or both), with zoom    |

## With an API key instead (scripts, CI)

If you would rather not sign in through the browser, create an API key in Homotope (“My account”,
“API keys”), keep it in an environment variable, and declare the server in your own
`~/.gemini/settings.json` instead of installing this extension:

```json
{
  "mcpServers": {
    "homotope": {
      "httpUrl": "https://homotope.app/mcp",
      "headers": { "Authorization": "Bearer $HOMOTOPE_API_KEY" }
    }
  }
}
```

Never write the key itself in a file you share or commit.

## Links

- Connect Homotope to an assistant: https://homotope.app/en/connectors.html
- API and MCP documentation: https://homotope.app/en/docs/api
- Privacy: https://homotope.app/en/privacy.html · Terms: https://homotope.app/en/terms.html
- Support: contact@homotope.fr

## Licence

MIT, see `LICENSE`.
