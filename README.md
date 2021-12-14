# understand.ai Anonymizer

## Examples

![License Plate Example Raw](images/coco02.jpg?raw=true "Title")
![License Plate Anonymized](images/coco02_anonymized.jpg?raw=true "Title")

![Face Example Raw](images/coco01.jpg?raw=true "Title")
![Face Example Anonymized](images/coco01_anonymized.jpg?raw=true "Title")


## Installation

To install the anonymizer just clone this repository, create a new **python3.9** environment and install the dependencies.  
The sequence of commands to do all this is

```bash
python -m venv ~/.virtualenvs/anonymizer
source ~/.virtualenvs/anonymizer/bin/activate

git clone https://github.com/understand-ai/anonymizer
cd anonymizer

pip install --upgrade pip
pip install -r requirements.txt
```

To make sure everything is working as intended run the test suite with the following command

```bash
pytest
```

Running the test cases can take several minutes and is dependent on your GPU (or CPU) and internet speed.  
Some test cases download model weights and some perform inference to make sure everything works as intended.


## Usage

In case you want to run the model on CPU, make sure that you install `tensorflow` instead of `tensorflow-gpu` listed
in the `requirements.txt`.

Since the weights will be downloaded automatically all that is needed to anonymize images is to run

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights
```

from the top folder of this repository. This will save both anonymized images and detection results as json-files to
the output folder.

### Advanced Usage

In case you do not want to save the detections to json, add the parameter `no-write-detections`.
Example:

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights --no-write-detections
```

Detection threshold for faces and license plates can be passed as additional parameters.
Both are floats in [0.001, 1.0]. Example:

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights --face-threshold=0.1 --plate-threshold=0.9
```

By default only `*.jpg` and `*.png` files are anonymized. To for instance only anonymize jpgs and tiffs, 
the parameter `image-extensions` can be used. Example:

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights --image-extensions=jpg,tiff
```

The parameters for the blurring can be changed as well. For this the parameter `obfuscation-kernel` is used.
It consists of three values: The size of the gaussian kernel used for blurring, it's standard deviation and the size
of another kernel that is used to make the transition between blurred and non-blurred regions smoother.
Example usage:

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights --obfuscation-kernel="65,3,19"
```
