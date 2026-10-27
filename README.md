# NKPI bilateral calculator — 9.1.1

This release starts each eye's inspector at its lowest supported mapped pachymetry and makes the measurement colors easier to read. The natural-eye view remains available, with OD and OS in one figure and all five point measurements together. Measurement views use opaque colors, numerical legends and a front-facing Map view. Each imported eye and visit supplies its own data; no example patient is embedded.

## Upload to GitHub

Extract **NKPI_v911_GitHub_Upload.zip** once. Upload these five files to the existing repository, replacing matching files.

| File | Location |
| --- | --- |
| `kcn_dual_calculator_app.py` | Repository root. This remains the Streamlit main file. |
| `nkpi_runtime.zip` | Repository root. **Keep this inner ZIP intact.** |
| `requirements.txt` | Repository root. |
| `README.md` | Repository root. |
| `.streamlit/config.toml` | Keep the `.streamlit` folder and this exact path. |

The four root files must be together, without another enclosing folder. Supporting application files are inside `nkpi_runtime.zip` and do not need individual upload. The launcher verifies the matching runtime and extracts it automatically.

In Streamlit, keep **Python 3.12** and **kcn_dual_calculator_app.py** as the main file. Reboot after the complete upload, refresh the page, and confirm **9.1.1-thinnest-maps** in the footer.

Open the original Pentacam ZIPs in the app again, verify the scalar values, and calculate. Fresh imports provide the accepted color calibration needed by this viewer. Older report objects may need re-importing; missing calibration is not guessed.

Local startup:

```bash
python -m pip install -r requirements.txt
python -m streamlit run kcn_dual_calculator_app.py
```

## Integrated cornea controls

- **OD and OS together:** linked rotation and zoom use equal physical axes and the same pixels per millimetre. The panels stack on narrow screens.
- **Whole surface / Section:** inspect the same surface or expose its available inner boundary. The section aligns with the direction of the lowest mapped thickness. It is a viewing cut, not tissue loss.
- **Map display:** natural appearance, anterior curvature, posterior curvature magnitude, thickness, anterior elevation, or posterior elevation. Quantitative colors are opaque and unlit, so the iris and lighting do not change their meaning. Legends show numbers, units and the selected value. Compatible eyes and visits share a stable scale; different definitions stay separate.
- **Map view:** one click presents the whole cornea from the front. From Natural eye, it opens Thickness when available, otherwise another available measurement. An already selected measurement is preserved. The 3D, Front and Profile controls remain available. Choosing another field keeps the selected point and camera.
- **Selected point:** each eye and visit starts at its lowest positive, supported mapped thickness. X/Y controls let you inspect other locations; **Thinnest point** returns to the minimum. All five fields refer to that same location, and missing values stay unavailable. Manual selections are remembered separately for each eye and visit.
- **Baseline / Latest:** each visit uses its associated report. A missing eye or visit stays unavailable.
- **Save both eyes:** one PNG includes the views, selected values, visit, and source limitations.

The on-eye callout follows the selected measurement. Curvature shows diopters, elevation shows signed micrometres, and Thickness/Natural eye shows pachymetry. Only a supported thickness-derived boundary uses a front-to-inner caliper. Curvature and elevation use a point marker and label.

The synthetic eye option and observed-change/evidence section remain removed. Regional comparisons, source reports, and cone evolution remain in the collapsed comparison area below the integrated viewer.

## What the surface represents

| Source | Geometry and measurements |
| --- | --- |
| Native anterior/posterior coordinates | Retained in their declared physical frame. Device pachymetry remains independent of the vertical surface gap. |
| Report elevation and verified reference sphere | Approximate anterior geometry. Available pachymetry offsets an inner boundary along the reconstructed anterior normal. |
| Partial readable report | Only supported patches, explicitly labeled. Clipped and unsupported regions stay open. |
| Maps without usable height data | Available values remain in the point inspector; patient surface geometry is marked unavailable. Curvature and summary scalars never create an invented cone. |
| No associated scan | An import/linking message with no patient measurements. |

Image values are estimates. Color intervals touching accepted calibration endpoints are excluded because endpoint colors can conceal out-of-range values. Original extracted grids remain unchanged for the existing regional calculations. Posterior elevation keeps its independent reference sphere and does not establish absolute posterior coordinates.

**Lowest mapped pachymetry** means the minimum among available supported samples in this viewer. With report images, local fitting, color resolution and missing regions can make it differ from the device's printed thinnest value. The scalar C is not used to invent a position or replace thickness at another coordinate. Missing pachymetry does not become zero. The lowest thickness point is not assumed to be the maximum curvature or maximum elevation point.

If a readable minimum is isolated from the surrounding reconstructed surface, its point and value remain available with a surface-gap label. The viewer does not move to a thicker point or add unsupported tissue to make the caliper look complete.

Curvature colors progress from cooler lower values to warmer higher values. Thickness reverses that direction so thinner areas are warmer. Signed elevation uses blue for negative values, a neutral zero and orange for positive values. These colors encode measurements, not diagnostic categories. Contour bands represent displayed numerical levels, not validated cone boundaries.

A smaller reconstruction aperture is never stretched over a larger cornea. The iris, sclera, and unmeasured periphery are anatomical reference. Report images cannot establish an exact 1:1 whole-eye replica. Lighting does not simulate patient refraction or vision.

See `docs/SURFACE_DATA.md` inside the runtime for the coordinate contract, processing rules, native import format, and limitations.

## Reliability and retained calculations

Trained models, coefficients, scalar calculations, bilateral import review, ranked input audit, and numerical/PDF exports are retained. Edited measurements, changed sources, eye removal, and next-patient clearing disconnect stale corneal data.

The matching **Tesseract.js 7.0.0** runtime, workers, supported LSTM cores, and English models are bundled. Original image ZIP OCR no longer needs a runtime download from a separate CDN. PDF support retains its existing loader requirements. Upstream licenses and checksums accompany the OCR assets.

The viewer adds no external texture service, API key, or JavaScript framework. Python declarations retain the tested `streamlit==1.50.0`, `reportlab==4.4.9`, and `numpy>=1.26.4,<3`. This release does not claim that every Python dependency is the newest published version.

See `docs/TEST_REPORT.md` for verification and its scope. Software checks establish implementation behavior, not clinical accuracy or surgical-planning validation.

Patient reports, extracted patient grids, photographs, and QA screenshots are excluded from this package.

## Bundle mismatch

Upload `kcn_dual_calculator_app.py` and `nkpi_runtime.zip` together from this same download, at the same repository level, preserving both names, then reboot. The launcher rejects a mismatched bundle instead of selecting another release.
