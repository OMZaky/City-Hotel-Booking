# City Hotel Booking

CSE382 machine learning project using the City Hotel (H2) booking dataset.

The project covers three tasks:

- Regression: predict the average daily rate (`ADR`).
- Classification: predict whether a booking is canceled (`IsCanceled`).
- Clustering: find and describe groups of similar bookings.

The preprocessing notebook loads the raw dataset, cleans its values, applies the
agreed row-removal rules, and saves the processed CSV. Models are still to be built.

## Setup

Clone the repository, then create a Python environment. On Windows:

```powershell
git clone https://github.com/OMZaky/City-Hotel-Booking.git
cd City-Hotel-Booking
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

The dataset is included at `data/raw/H2.csv`.
Open [preprocessing.ipynb](notebooks/preprocessing.ipynb) in VS Code and select
the `.venv` kernel to run locally. The default `DATA_SOURCE = "repo"` reads the raw
CSV directly from this public GitHub repository. Set `DATA_SOURCE = "local"` to
use the included file without a network connection; change `LOCAL_DATA_PATH` if
your file is elsewhere.

The processed CSV includes `ArrivalDate`, combining the arrival year, month, and
day. The original date components remain available. CSV files do not preserve
datetime types; parse the date when reloading:

```python
df = pd.read_csv("data/processed/H2.csv", parse_dates=["ArrivalDate"])
```

the study covers overnight booking records with at least one adult, including
canceled bookings. zero-adult and zero-night records are excluded by scope.
zero rates, repeated rows, and repeat-history combinations are retained within
that scope. final
reservation status and its date are excluded from the processed file after the
status consistency check.

the shared csv contains targets as well as possible inputs. separate the target
for each task and check feature availability at the intended prediction time.
the full table keeps analysis columns; use the shared split files for supervised
experiments. prediction is assumed to happen when the reservation is entered.

the notebook also exports these files under `data/processed/`:

| file | purpose | rows |
| --- | --- | ---: |
| `train.csv` | initial fitting before july 2016 | 31,007 |
| `validation.csv` | selection using july-december 2016 entries | 9,272 |
| `test.csv` | final evaluation on january-april 2017 entries | 13,321 |
| `final_train.csv` | refit the selected setup using eligible pre-test records | 50,912 |

each split has 18 approved inputs and both targets. remove both target columns
from inputs, regardless of the task:

```python
train = pd.read_csv("data/processed/train.csv", parse_dates=["ArrivalDate"])
X_train = train.drop(columns=["ADR", "IsCanceled"])
y_regression = train["ADR"]
y_classification = train["IsCanceled"]
```

calendar cutoffs use estimated booking-entry dates and conservative outcome
availability. deferred records remain in `H2.csv`; the final refit pool includes
earlier entries whose outcomes have become available. the source's later snapshots
and limited arrival window remain evaluation limitations.

fit imputation, scaling, encoding, feature selection, and resampling within
training splits. keep the final test out of selection and tuning. later
cross-validation should use expanding calendar windows with outcome checks.

fixed parsing, missing-value labels, and scope rules run before splitting.
historical label-quality checks are separate from live prediction inputs.
the split is created before exploration; all notebook summaries and plots use
only the initial training rows. full-data plots were inspected earlier; the switch
to training-only exploration does not undo that exposure.

## Colab in VS Code

Install the official [Google Colab extension](https://marketplace.visualstudio.com/items?itemName=google.colab).
Open the notebook, choose **Select Kernel > Colab**, and sign in with your Google account.
Start with a CPU runtime.

Keep `DATA_SOURCE = "repo"`. The notebook reads the raw CSV directly from GitHub,
so no Drive mount or separate dataset upload is needed.

Restart the kernel and run the notebook cells from top to bottom. Your local files
and `.venv` are separate from the Colab runtime. If dependencies need installing remotely,
upload `requirements.txt` using **Upload to Colab**, then run:

```python
%pip install -r /content/requirements.txt
```

Restart the kernel if prompted. The cleaned file is saved to
`/content/data/processed/H2.csv` when Colab uses its usual working directory.
Download the result or copy it to persistent storage before ending the session.

## Folders

- `src/hotel_ml/`: reusable preprocessing, features, task code, evaluation, and inference.
- `notebooks/`: exploration and experiments, grouped by task.
- `configs/`: experiment settings, grouped by task and version.
- `data/`: the included H2.csv and ignored processed datasets.
- `outputs/`: generated experiment results; contents stay out of Git.
- `reports/`: selected figures and milestone reports.
- `tests/`: checks for the code we add.

Most folders are empty for now. Their `.gitkeep` files only make them visible in Git.
The [course brief](docs/CSE382_Project_Description_Milestones.pdf) has the full requirements.
