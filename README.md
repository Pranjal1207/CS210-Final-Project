### Repository overview

| File                     |  Role                                                                                                                                                                                                                                                                                                     | How to run it                                                                                                                                                                                                                                                                                                                                                                                              |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`stockdb.sql`**        | Builds the project’s MySQL schema—four tables (`stocks`, `prices`, `dividends`, `splits`) with primary‑, foreign‑ and unique‑key rules so duplicates can’t slip in .                                                                                                                                      | 1. Launch **MySQL Workbench** (or `mysql` CLI). 2. Open the script and execute it; a database named **`stockdb`** will appear.                                                                                                                                                                                                                                                                             |
| **`stockreprice.ipynb`** | End‑to‑end ETL + model notebook. ‑ Reads the raw CSVs in `~/Downloads/stocks/`.<br>‑ Cleans/renames columns, writes them into `stockdb`.<br>‑ Creates three features (1‑day return, 20‑day SMA, 14‑day RSI).<br>‑ Trains a 300‑tree **Random Forest** for each ticker and pickles the model to `models/`. | 1. Open **Anaconda Navigator → Environments → Create**; pick Python 3.10.<br>2. With the env selected, click **Open Terminal** and run:<br>`conda install -c conda-forge pymysql scikit-learn plotly yfinance ipywidgets`<br>3. Still inside Navigator, click **Launch → Jupyter Notebook**.<br>4. Navigate to `stockreprice.ipynb`, update `DATA_DIR` and the MySQL password if needed, then **Run All**. |
| **`newdashboard.ipynb`** | Interactive analysis notebook (ipywidgets + Plotly). It: 1) pulls cleaned data from MySQL, 2) reloads the pickled forest, 3) plots the last 30‑‑365 closes with a red‑star next‑day prediction, and 4) prints the absolute‑percentage error if the actual close is in the table.                          | 1. In the same Jupyter session, open `newdashboard.ipynb`.<br>2. Run the first cell to load dependencies and connect to MySQL.<br>3. Use the **Ticker** dropdown and **Days** slider to explore results.                                                                                                                                                                                                   |

### Quick start in 6 steps

1. **Clone the repo**
   `git clone https://github.com/your‑handle/stock‑forecast‑demo.git`

2. **Create the Conda environment**
   Open **Anaconda Navigator → Environments → Create** (name it *stocks*).

3. **Install dependencies**
   With *stocks* highlighted, press **Open Terminal** and run:

   ```
   conda install -c conda-forge pymysql scikit-learn plotly yfinance ipywidgets
   ```

4. **Provision MySQL**

   * Start MySQL locally.
   * Open `stockdb.sql` in Workbench and execute.

5. **Run `stockreprice.ipynb`**

   * In Navigator, click **Launch → Jupyter Notebook**.
   * Open the notebook, tweak credentials, **Run All**.
   * Models save to `models/`.

6. **Explore `newdashboard.ipynb`**

   * Run the notebook, choose a ticker, slide the day window, and inspect the chart and printed error.

That’s it—clean schema, reproducible ETL, trained forests, and an interactive dashboard, all from inside Anaconda Navigator.
