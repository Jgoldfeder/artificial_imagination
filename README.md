# Grounded Imagination Leads to Adaptation

**EvoAug: Evolutionary Search over Generative Augmentation Trees for Few-Shot Learning**

Judah Goldfeder¹, Shreyes Kaliyur¹, Vaibhav Sourirajan¹, Patrick Minwan Puma², Philippe Martin Wyder³, Yuhang Hu¹, Jiong Lin¹, Hod Lipson¹

¹Columbia University, ²Harvard University, ³University of Washington

## Abstract

Imagination — the capacity to simulate plausible unseen variations of the world — is a fundamental mechanism of biological adaptation. In data-scarce environments, organisms that can mentally rehearse novel scenarios gain decisive advantages over those limited to direct experience.

We argue that adaptive imagination requires two components: a generative world model capable of generating plausible novel scenarios, and a selection process that keeps those scenarios grounded and relevant. Neither alone is sufficient.

We present **EvoAug**, a computational model of this principle: an automated pipeline that uses generative models (ControlNet diffusion and Zero123 NeRF) to synthesize task-relevant visual variations, and evolutionary search to ensure those simulations remain grounded and adaptive. Without evolutionary selection, unconstrained generative augmentation degrades performance — the computational analog of maladaptive fantasy. With it, the system discovers augmentation strategies that recover domain structure without explicit supervision, even from a single example per class.

Results across six fine-grained classification datasets demonstrate that combining a generative world model with evolutionary grounding outperforms classical augmentation strategies, with gains largest precisely where data is scarcest — the conditions under which imagination matters most.

## Setup

1. Install conda:
```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
```

2. Add to `.bashrc`: `export PATH="/home/<username>/miniconda3/bin:$PATH"`

3. Create and activate environment:
```bash
conda create --name diffaug python=3.10.15
conda install pip
conda activate diffaug
```

4. Install dependencies:
```bash
# Run the comments at the top of requirements.txt first
pip install -r requirements.txt
```

5. (Optional) Set up Jupyter kernel:
```bash
conda install -c conda-forge ipykernel
python -m ipykernel install --user --name=diffaug
```

6. Download model weights:
```bash
./models/download_models.sh
huggingface-cli download runwayml/stable-diffusion-v1-5 --local-dir ./models/runwayml-stable-diffusion-v1-5 --local-dir-use-symlinks False
```

## Usage

The primary script is `genetic_algorithm.py`. Key flags:

| Flag | Description |
|------|-------------|
| `--num_generations` | Number of evolutionary generations |
| `--sol_per_pop` | Population size |
| `--num_parents_mating` | Number of parents for crossover |
| `--dataset` | Dataset (caltech256, flowers102, stanford_dogs, stanford_cars, oxford_pets, food101) |
| `--num_ways` | Number of classes (N-way) |
| `--num_shots` | Samples per class (K-shot) |
| `--tree_depth` | Maximum augmentation tree depth |

### Example

```bash
python genetic_algorithm.py \
    --num_generations 10 \
    --sol_per_pop 14 \
    --num_parents_mating 6 \
    --keep_elitism 1 \
    --keep_parents 1 \
    --mutation_percent 10 \
    --dataset caltech256 \
    --num_ways 5 \
    --num_shots 2 \
    --subset 42 \
    --model_type resnet50 \
    --tree_depth 3 \
    --num_augmentations_per_image 5 \
    --num_iterations_for_val 20 \
    --num_iterations_for_test 200 \
    --seed 42
```

## Citation

```bibtex
@inproceedings{goldfeder2025imagination,
  title={Grounded Imagination Leads to Adaptation},
  author={Goldfeder, Judah and Kaliyur, Shreyes and Sourirajan, Vaibhav and Puma, Patrick Minwan and Wyder, Philippe Martin and Hu, Yuhang and Lin, Jiong and Lipson, Hod},
  booktitle={ALIFE 2026},
  year={2026}
}
```
