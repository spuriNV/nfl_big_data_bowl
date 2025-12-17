# Interceptability Field (IF) + Window of Opportunity (WOO)
## NFL Big Data Bowl 2026 - Analytics Competition Submission

### Overview

This project implements a novel spatial-temporal framework for analyzing player movement during pass plays in the NFL. The **Interceptability Field (IF)** quantifies each player's probability of reaching any point in space before the ball arrives, while the **Window of Opportunity (WOO)** provides a single-number summary that coaches can instantly understand.

### Key Features

- **Interceptability Field (IF)**: Spatial probability field showing who can reach the ball first
- **Window of Opportunity (WOO)**: Single-number metric (Receiver_IF - Defender_IF) that quantifies catch window
- **Temporal Analysis**: Tracks how windows evolve over time
- **Player Evaluation**: Identifies elite defenders and receivers
- **Scheme Analysis**: Evaluates offensive and defensive strategies
- **Beautiful Visualizations**: Heatmaps, timeseries, and field overlays

### Files

- `interceptability_field_analysis.ipynb` - Main analysis notebook with all code and examples
- `KAGGLE_WRITEUP.md` - Complete writeup for Kaggle submission (under 2000 words)
- `supplementary_data.csv` - Pass results, routes, and coverage data
- `train/` - Input and output tracking data for 18 weeks
- `README.md` - This file

### Visualizations

The project includes 10 high-quality visualizations (media_*.png files):

1. **media_woo_timeseries.png** - WOO evolution over time for example plays
2. **media_woo_distribution.png** - Distribution of WOO Peak values across all plays
3. **media_woo_by_outcome.png** - WOO metrics grouped by pass outcome (Completion/Incomplete/Interception)
4. **media_if_field.png** - Interceptability Field heatmap showing player reachability
5. **media_if_field_polished.png** - Polished IF field visualization with NFL field background
6. **media_route_coverage_analysis.png** - WOO Peak by route type and coverage scheme
7. **media_route_coverage_heatmap.png** - Route × Coverage interaction heatmap
8. **media_route_coverage_extremes.png** - Best and worst route-coverage combinations
9. **media_temporal_trends.png** - WOO metrics across early, mid, and late season
10. **media_player_comparison.png** - Player-level analysis showing top defenders by IF Delta

These visualizations demonstrate the framework's capabilities and key findings from the analysis of 180 plays across 18 weeks.

### Quick Start

1. **Open the notebook**: `interceptability_field_analysis.ipynb`
2. **Run all cells** to load dependencies and functions
3. **Analyze a play**:
   ```python
   results, play_df, times, ball_x, ball_y = analyze_play(
       'train/input_2023_w01.csv', 
       game_id=2023090700, 
       play_id=101
   )
   ```
4. **Visualize results**:
   ```python
   plot_woo_timeseries(results['woo_values'], results['frame_ids'])
   ```

### Methodology

#### Interceptability Field (IF)

For each player *i* and field coordinate *(x, y)*:

**IF_i(x, y) = P(player i can reach (x,y) before ball hits turf)**

This incorporates:
- Current speed and acceleration
- Direction and orientation
- Distance to target
- Time available (from ball trajectory)
- Realistic change-of-direction constraints
- Reaction delay (~0.2 seconds)

The calculation uses kinematic equations: d = v₀t + ½at², with maximum speed constraints and turn time penalties.

#### Window of Opportunity (WOO)

**WOO = Receiver_IF - max(Defender_IF)**

- **WOO > 0**: Offense advantage (receiver more likely to reach ball)
- **WOO < 0**: Defense advantage (defender more likely to reach ball)
- **WOO = 0**: Neutral window

The WOO curve over time shows how the window opened or closed, providing insights into early window advantage, late window closure, and peak window timing.

### Key Results

Analysis of 180 plays across 18 weeks reveals:

- **Outcome Validation**: Completions show 1.78x larger WOO Peak than incompletions (0.108 vs 0.061)
- **Route Analysis**: Short routes (FLAT, SCREEN, SLANT) create 69% larger windows than deep routes
- **Coverage Analysis**: Man coverage creates 2.3x larger windows than Cover-2 Zone
- **Temporal Trends**: Windows are 81% larger in early season, suggesting defenses improve over time
- **Player Analysis**: Top defenders show IF Delta > 0.40, indicating elite closing speed

### Applications

1. **Player Scouting**: Identify elite defenders who close windows faster
2. **Team Evaluation**: Compare schemes and route effectiveness
3. **QB Analysis**: Evaluate throw quality and decision-making
4. **Role Comparison**: Compare positions (CB vs Safety, etc.)

### Technical Details

- **Physics-based**: Uses kinematic equations with realistic constraints
- **Reproducible**: Deterministic calculations based on tracking data
- **Robust**: Works on every play with no special cases
- **Interpretable**: Transparent methodology, no black-box ML
- **Validated**: Correlates with known outcomes (completions, interceptions)

### Requirements

```
numpy
pandas
matplotlib
seaborn
scipy
tqdm
```

### Citation

If you use this work, please cite:
```
Interceptability Field (IF) + Window of Opportunity (WOO)
NFL Big Data Bowl 2026 - Analytics Competition
```

### Contact

For questions or collaboration, please refer to the Kaggle competition page.
