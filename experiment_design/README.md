# Experiment design

The exact items used in each experiment: which image or pair, which piece, the true and false claims, and the option order.
They contain only identifiers and labels (no captions, no images). Identifiers refer to the public datasets: `tangram_name` and
`annotation_idx` to KILOGRAM, and `pair_id` / image ids (for example `AB_001`, `AB_001_safe`) to VERI-Emergency.

## Tangram

| File | One row per | Columns |
|---|---|---|
| `tangram_shape_probe_items.csv` | item of the shape probe (the same 100 items are used in the two-turn self-consistency experiment) | `item_id`, `image_id`, `tangram_name`, `annotation_idx`, `n_colors` (distinct piece colors, 6 or 7), `probe_piece_id` (1 = square, 2 = parallelogram), `probe_color`, `true_shape`, `false_shape` (the shape named by false assertions), `option_order` (the five answer options, in the order shown, JSON) |
| `tangram_part_match_items.csv` | image of the part-match experiment | `image_id`, `tangram_name`, `annotation_idx`, `whole` (the annotator's label for the whole shape), `part` (the labeled part to match), `swap_in_part` (another part of the same figure, whose piece the false assertion names), `correct_item`, `wrong_item` (color and shape of the piece), `option_order` (the pieces listed in the prompt, JSON) |
| `tangram_three_turn_items.csv` | item of the three-turn trust-building experiment | `item_id`, `image_id`, `tangram_name`, `annotation_idx`, `n_colors`, `turn_order` (the shape asked about in each turn), `triangle_turn` (which turn asks about the triangle), `option_order`, and for each turn `k`: `turn_k_piece_id`, `turn_k_color`, `turn_k_true_shape`, `turn_k_false_shape` |

In the three-turn experiment all three turns use the same image and each turn asks about a different piece, identified by a color no other piece has.

## VERI

| File | One row per | Columns |
|---|---|---|
| `veri_pairs.csv` | safe / dangerous pair | `pair_id`, `category`, `safe_image_id`, `danger_image_id`, `danger_is_image_1_in_safe_vs_danger` (whether the dangerous image is shown first in the one-turn safe vs. danger experiment) |
| `veri_self_consistency_items.csv` | item of the two-turn experiment (each pair once) | `item_id` (pair), `danger_position` (1 or 2), `assertion_target` (`safe` or `danger`: which image the assertions are about), `target_position`, `true_label`, `false_label` (the labels a true and a false assertion give that image), `image_1_id`, `image_2_id` |
| `veri_trust_building_items.csv` | item of the three-turn experiment | `item_id`, `pair_order` (the three pairs in turn order), and for each turn `k`: `turn_k_pair_id`, `turn_k_danger_position`, `turn_k_assertion_target`, `turn_k_target_position`, `turn_k_true_label`, `turn_k_false_label` |

In the three-turn experiment each turn shows a different pair; every pair is the first pair of exactly one item, the second of exactly one,
and the third of exactly one.

## Notes

* `veri_pairs.csv` gives the design of the one-turn safe vs. danger experiment. The outputs of Qwen3 (the pairs it completed), Mistral, GPT-4o and GPT-5-mini
  agree with it on every pair. In the Gemini run, pair `AB_001` was shown with the dangerous image first instead (all 21 of its trials); every other pair follows the design.
* Each list was checked against the saved outputs (the items, option orders and assertion labels recorded in the result files).
