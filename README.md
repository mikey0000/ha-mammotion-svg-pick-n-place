# Mammotion SVG Pick and Place

An interactive Lovelace card for placing, editing, and deleting SVG pattern tiles on your Mammotion mower's map directly from the Home Assistant dashboard.

## Installation via HACS

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=mikey0000&repository=ha-mammotion-svg-pick-n-place&category=plugin)

Or manually:

1. Open HACS → three-dot menu (⋮) → **Custom repositories**
2. Add `https://github.com/mikey0000/ha-mammotion-svg-pick-n-place` with category **Dashboard**
3. Find **Mammotion SVG Pick and Place** and click **Download**

## Requirements

The **Mammotion** integration must be installed and at least one mower must have a synced map (run a map sync from the app if needed).

## Adding the card

In your Lovelace dashboard → **Edit** → **Add Card** → **Custom: Mammotion SVG Map Aligner**.

Or paste the YAML manually:

```yaml
type: custom:mammotion-svg-card
entity: lawn_mower.my_luba2   # optional: pre-selects this mower on load
device_type: "2.5"            # "2.5" = Luba 1 / Yuka (default), "4.0" = Luba 2
card_height: 600              # optional height in px (default 600)
```

The `entity` field is optional. If omitted, the card shows a **Mower** dropdown populated from all `lawn_mower.*` entities in HA — useful if you have multiple mowers.

## How to use

1. **Select a mower** from the dropdown (auto-populated from all `lawn_mower.*` entities in HA).
2. **Select an area** from the area dropdown — the mower map renders with all areas shown.
3. **Load an SVG** — drag a `.svg` file onto the drop zone, click to browse, or paste SVG markup directly.
4. **Position the tile** — drag it on the map. Blue circle = move, green circle (SE corner) = scale, amber circle (top) = rotate.
5. Fine-tune position, scale, rotation, and dimensions in the **Transform** panel.
6. Click **Send to Device** — the tile is chunked and sent via the `mammotion.svg_add` service. The device-assigned hash is shown on success.
7. Use the **Existing** tab to see tiles already on the device. Click **Edit** to load a tile back into the Place tab for updating, or **Delete** to remove it.
8. Click **Refresh** at any time to reload the map data from the device.

## Services

The card uses these HA services (also callable directly from **Developer Tools → Services**):

| Service | Description |
|---|---|
| `mammotion.get_map_data` | Returns raw area/SVG data for use by the card |
| `mammotion.svg_add` | Send a new SVG tile → returns `device_hash` |
| `mammotion.svg_update` | Replace an existing tile by `device_hash` |
| `mammotion.svg_delete` | Remove a tile by `device_hash` |

All services take `entity_id` (any Mammotion entity for the target mower) plus operation-specific fields. See the integration's **Services** page in Developer Tools for full field documentation.
