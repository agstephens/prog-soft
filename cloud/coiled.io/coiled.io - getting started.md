## Useful links
- https://www.coiled.io/
- https://docs.coiled.io/user_guide/usage/functions/index.html
- [Coiled from scratch](https://youtu.be/gD_5LiInXmE)
## Getting started
They say you get the first n x 1000 units for free.

1. Sign up (I used my NCAS Google Account)
2. It took me here: https://cloud.coiled.io/get-started
3. Create a virtual env and install coiled (in this case on Windows):

```shell
python -m venv venvs\venv-coiled.io
.\venvs\venv-coiled.io\Scripts\Activate.ps1
pip install coiled "dask[complete]"
```

4. Login (authenticate) to coiled:
```shell
coiled login
```

5. Grant `coiled` access to your AWS account:
   - click the button
   - took me here: https://cloud.coiled.io/settings/setup/credentials

6. Run a script that calls out to the cloud (or runs in the cloud):
```shell
$ coiled run python mypy.py
```

## How does it work?
`coiled` will detect your `conda` and `pip` environment (including local libraries) and will copy everything it needs to up to the cloud instance(s), and then run them. 

## Example 1: a python script running remotely
```shell
coiled run --vm-type m6i.32xlarge python parallel_script.py
```

## Example 2: a coiled function decorator
```python
import coiled
import pandas as pd

df = pd.read_csv("data.csv")  # Read local data

@coiled.function(memory="512 GB", vm_type="p3.2xlarge")  # This function runs remotely
def process(df):
    df = df[df.name == "Alice"]
    return df

result = process(df)          # Process on remote VM
print(result)
```