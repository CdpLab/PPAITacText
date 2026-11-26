<h1 align="center">
Lightweight Tactile Texture Modeling based on Physical Priors and Adaptive Inference
</h1>

<p align="center">
Tianyu Fan, Zhangjia Deng, Xincheng Xia, and Dapeng Chen*<br>
Nanjing University of Information Science and Technology
</p>

---

<h2 align="center">ABSTRACT</h2>
Tactile vibrations vary with surface properties and interaction states, reflecting recent contact states and current scanning conditions. This study proposes a lightweight tactile texture modeling method integrating physical priors and adaptive inference to incorporate these conditions into temporal feature updates while limiting computational cost. The model uses historical three-axis vibrations, texture images, scanning velocity, motion direction, and normal force to predict three-axis vibrations over the next 20 ms. Historical vibrations are divided into temporal patches and processed by a Transformer. Gated fusion combines visual representations and physical priors into a unified conditioning representation. Multi-layer feature-wise linear modulation (Multi-FiLM) generates layer-specific scaling and shifting parameters. Training jointly constrains time-domain waveform errors and spectral magnitude differences. On the standard validation split of the Cluster Haptic Texture Dataset, Multi-FiLM achieves a median root mean square error (RMSE) of 0.0277 g, lower than those of additive fusion and single-layer FiLM. On unseen textures, its median RMSE, averaged across three random seeds, is approximately 3.6% lower than that of the baseline Transformer. On an independent set of repeat recordings, the model achieves a median RMSE of 0.0273 g and a median dynamic correlation of 0.8374. With a batch size of one and FP16 mixed precision on an RTX 3090 GPU, the online network achieves a median forward-pass time of 0.771 ms. These results indicate improved prediction performance over the compared fusion configurations and lower errors on unseen textures, with low online computational cost.

---

# Lightweight tactile texture modeling

Companion repository for *A Study on Lightweight Tactile Texture Modeling with Physical Priors and Adaptive Inference* by Tianyu Fan, Zhangjia Deng, Xincheng Xia, and Dapeng Chen.

The model uses 160 ms of measured three-axis vibration history together with scanning velocity, motion direction, normal force, and texture image features to predict the next 20 ms of vibration at 8 kHz. A Transformer encodes the vibration history. Layer-specific FiLM parameters condition its temporal representations on the physical and visual inputs.

## Release status

This revision includes 11 Python source files covering preprocessing, training, and evaluation. Environment specifications and model weights are pending. End-to-end reproduction from a clean checkout has yet to be verified.

## Verified artifact paths

Paths below are relative to the experiment project root. Run the original scripts from that directory when reproducing these paths.

- `build_cluster_H1_leakage_safe_manifest.py` writes `cluster_H1_results/H1_protocol_A_manifest.csv`, the manifest read by the 8 kHz cache builder. The original manifest builder contains a machine-specific output path.
- `build_cluster_swin_multistage_features.py` writes `cluster_visual_features/swin_t_multistage_cropmean_features.npz`.
- `run_FINAL_TEST_visual.py` reads the evaluation-window index at `cluster_FINAL_TEST_results/final_test_eval_index.csv` and loads `cluster_I8_gated_visual_residual_results/logmag/lambda_0.01/seed_42_full/best_frequency.pt`.

The generator `build_final_test_index.py` writes the final evaluation index. The verified experiment index contains 9,440 distinct records from repeat 1 of the test split, with three windows per record (28,320 windows). All 28,320 window keys are unique. The index includes window boundaries and source/cache paths. The paths document the verified experiment layout; the artifacts themselves are not included in this revision.
