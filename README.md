# City Hotel Booking

CSE382 machine learning project using the City Hotel (H2) booking dataset.

The project covers three tasks:

- Regression: predict the average daily rate (`ADR`).
- Classification: predict whether a booking is canceled (`IsCanceled`).
- Clustering: find and describe groups of similar bookings.

We're starting with data preparation. The notebook currently loads the dataset;
cleaning, feature engineering, and models are still to be built.

## Setup

Clone the repository, then create a Python environment. On Windows:

```powershell
git clone https://github.com/OMZaky/City-Hotel-Booking.git
cd City-Hotel-Booking
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Put your copy of `H2.csv` in `data/raw/`. The dataset is not included in Git.
Open [preprocessing.ipynb](notebooks/preprocessing.ipynb) in VS Code and select
the `.venv` kernel to run locally.

## Colab in VS Code

Install the official [Google Colab extension](https://marketplace.visualstudio.com/items?itemName=google.colab).
Open the notebook, choose **Select Kernel > Colab**, and sign in with your Google account.
Start with a CPU runtime.

For the existing Drive cell, put the dataset at:

```text
My Drive/hotel-booking-project/data/H2.csv
```

Run the setup, Drive, and loading cells in order. Your local files and `.venv`
are separate from the Colab runtime. If dependencies need installing remotely,
upload `requirements.txt` using **Upload to Colab**, then run:

```python
%pip install -r /content/requirements.txt
```

Restart the kernel if prompted. Save generated results to Drive before ending a session.

## Folders

- `src/hotel_ml/`: reusable preprocessing, features, task code, evaluation, and inference.
- `notebooks/`: exploration and experiments, grouped by task.
- `configs/`: experiment settings, grouped by task and version.
- `data/`: original and processed datasets; contents stay out of Git.
- `outputs/`: generated experiment results; contents stay out of Git.
- `reports/`: selected figures and milestone reports.
- `tests/`: checks for the code we add.

Most folders are empty for now. Their `.gitkeep` files only make them visible in Git.
The [course brief](docs/CSE382_Project_Description_Milestones.pdf) has the full requirements.

## Working together

All five members use their own GitHub accounts. Create a branch for your work,
make small commits, and open a pull request when it's ready. Merging into `main`
requires one teammate's approval.

Coordinate before editing the same notebook, clear its outputs before committing,
and keep datasets and fitted models out of Git.
