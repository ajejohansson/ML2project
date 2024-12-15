# ML2project
Andreas Johansson repository for LT2326 final project

Content (as of submission):

project.ipynb - main code, notebook designed to be runnable as a script

data/
- chaosNLI - project testsets, also used as training data for toy run
  - generated through terminal commands from here: https://github.com/easonnie/ChaosNLI, not setup in code

- NLI_variation - testset from here: https://github.com/epavlick/NLI-variation-data
  - setup in notebook w/ requests library
   
not in submission due to size, all training sets:
- Stanford NLI
- MultiNLI
- Adversarial NLI
- All built from terminal from here: https://github.com/facebookresearch/anli
- All assumed to be local for non-toy run

models/
- bert-base-uncased-bs32-eps6-lr5e-05:

Model directory used for project report

Contains a filesystem with:
- a directory 'r'+n per round n, a sibling 'results' directory for cross-round results, a json file with hyperparameters used for training
- each 'r' dir has subdirs 'e'+n per epoch with a sibling 'results' directory for in-round results
- each 'e' dir has various files, mostly huggingface-style model files for the corresponding model checkpoint (safetensors removed for git submission due to size), and a 'results' directory for per-epoch results
            
All results are in plots

The project can generate new filesystems with the same structure into models/

Running instructions:


The project code is in a notebook (project.ipynb) designed to be able to be run as a script.

Quickrun instructions: run all of the notebook with config as submitted. Will create a directory "toy-models" in /models with the same general filesystem structure as for bert-base-uncased-bs32-eps6-lr5e-05 above, except with fewer rounds/epochs, and with present model checkpoints. A second run will create "toy-models_2, etc. Note that toy runs use both toy hyperparameters and data, and clearly create bad models.

(The rest only matters for modular runs, which may not be necessary)

Config explanation (excluding the obvious, like 'device'):

"toy_run": whether notebook will use toy parameters and data. Must be truthy since 1. non-toy training data is not in repository, and 2. new full models would take too long to train anyway.

"train_eval_mode": Set to "train", will only train model and save checkpoints. Set to "eval", will only evaluate. Anything else, from "both" to "schackbräde", will do both. Run first with "train" then with "eval" (with no other config changes, which is ill-adviced) is equivalent to one full run

"manual_modelname": specifies the name of the model directory (/models nearest subdirectory string, no additional path information) that will be created and/or evaluated. If falsy, will use a default model naming scheme, e.g. "toy_models" for toy runs. If "train_eval_mode" = "eval", should be the name of an existing directory in /models, otherwise any string or any false value.

"overwrite": If truthy, training will overwrite the directory of the old model with the designated name if it exists (whether that be a default name or a manual name). Since non-toy runs default names use hyperparameter information for name constructions, will overwrite a model run on the same hyperparameters. If falsy, will create a name variant if name already exists, e.g. "toy-model" --> "toy-model_2". No effect if "train_eval_mode" = "eval"

"model_id": selects which huggingface model to use (see globals() function, model_selection dict). Only Bert base is used for the project, but others are feasible (possibly with some slight modifications). Not tested to work with all current code, but it should work in theory.

"manual_hyperparams": if the default toy or regular hyperparameters (see globals()) are not suitable, this can overwrite either. Set subfield "use" to something truthy to use the rest of the subfields instead of the default ones. "toy_run" must still be set to the desired mode regardless of "manual_hyperparams" values, as it also affects other parts of the code. The reason for modular hyperparameters not being the default is that one training session takes many hours as it is, which makes it unfeasible to experiment with


