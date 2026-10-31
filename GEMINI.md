# Homotope

Homotope vectorizes raster images (PNG, JPEG) into clean vector files through its MCP server.

- Every call to `vectorize_image` is a **server job**: the image is sent to Homotope and the job
  counts against the user's Pro quota, then credits. Tell the user before sending an image, and
  never retry a refused job (402, 429) in a loop.
- `vectorize_image` returns at once with a `job_id`: poll `get_job` every few seconds until the
  state is `done`, then call `get_svg`, or `get_result` for PDF, EPS or DXF.
- Look before recomputing: `get_preview` returns a PNG of the finished job (result, source or
  both side by side, `zoom` on a `check_cut` position); it is free, a new `vectorize_image` is not.
- For cutting (laser, vinyl, plotter) or printing, call `check_cut` on the finished job (with the
  real width in mm) and report the blocking problems before the user sends the file; its `advice`
  names the setting to change for the next `vectorize_image` (at most 3 jobs per image).
- For stickers, ask `vectorize_image` for a cut contour (margin, gap bridging, one sticker or
  separate ones): the SVG, PDF and DXF then carry a CutContour layer that follows the outer
  silhouette and ignores inner holes; `get_result` can return that cut line alone as a DXF.
- Pick the preset that matches the image: `logo` for logos and flat artwork, `photo` for photos
  and posters, `centerline` for a single-stroke trace of line drawings. Unsure? Call
  `suggest_preset` first (free, no job), or pass preset "auto" to `vectorize_image`.
- To try another preset on the same image, pass the earlier job's id as source_job_id instead of
  sending the image again: it is still one job, and the same image is computed at most a few times
  an hour. Look at the result before computing again.
- The full playbook (suggest, compute, look and check, correct with the advice; when to stop) is
  served by the server itself: run the prompt `/homotope-workflow` (optional `--goal`, `--width_mm`)
  or read the resource `homotope://agents`; also at https://homotope.app/en/agents.md
- To vectorize without sending anything, the user can use the free workshop in the browser:
  https://homotope.app/atelier
