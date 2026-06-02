# MaxInsights Intern — Paper Digest

An interactive paper-reading dashboard for egocentric computer vision research, covering body & hand pose estimation, large-scale datasets, and field trends.

**Live site**: open `index.html` in any browser (no build step required).

---

## Coverage

| Topic | Papers |



|---|---|
| **Public Datasets** | NymeriaPlus, Nymeria, EPIC-KITCHENS, Ego-Exo4D v1/v2 |
| **SMPL-X Body Regression** | HMD-Poser, EgoEgo, EgoAllo, EgoPoseFormer v2 |
| **Hand Regression (MANO)** | HaMeR, WiLoR, HaWoR, Dyn-HaMR |
| **CVPR Trends** | Ego Full-Body Estimation (CVPR 2026+), Works Using Ego-Exo4D |
| **Major Research Groups** | Project Aria (Meta Reality Labs), non-Meta egocentric groups overview |
| **Future Work** | EgoSMPL-X unified body+hand+face proposal |
| **Tools** | Viser (imperative web-based 3D visualization) |

## Structure

```
index.html          # Main SPA: sidebar paper list + iframe viewer
subpages/           # One HTML file per paper / topic summary
pdfs/               # Local PDF copies
```

## Usage

1. Clone the repo and open `index.html` in a browser.
2. Use the **search bar** to filter by paper name or keyword.
3. Click any entry in the sidebar to load the full digest in the right panel.
4. ArXiv and PDF links are embedded in each paper page.

## Adding a Paper

在 Claude 或 Codex 中直接说：

> 根据以上模版，给我增加以下论文总结：`<论文名称 / arXiv 链接>`
