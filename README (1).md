# 💊 AMR-Guard — AI Antibiotic Stewardship Assistant

**Theme:** Combating Antimicrobial Resistance (Swasth Bharat)
**Type:** Working AI prototype (Python + Machine Learning + Web app)

AMR-Guard predicts which antibiotics are likely to work for a patient's infection
*before* the lab report comes back, and recommends the **narrowest safe antibiotic**
using the WHO **AWaRe** (Access / Watch / Reserve) framework.

> ⚠️ Educational prototype trained on synthetic data. Not for clinical use.

---

## 1. Project files (what each one does)

| File | Job |
|---|---|
| `config.py` | All medical constants: organisms, antibiotics, AWaRe groups, resistance rates |
| `generate_data.py` | Creates 6,000 realistic synthetic culture reports → `data/amr_isolates.csv` |
| `train_model.py` | Trains one AI model per antibiotic, picks the best algorithm, saves to `models/` |
| `stewardship.py` | Turns predictions into a recommendation (Access → Watch → Reserve rule) |
| `app.py` | The web app you demo to judges (Streamlit) |
| `demo_cli.py` | Backup demo in the terminal — no browser needed |
| `requirements.txt` | Python libraries to install |
| `docs/` | Project report + 7-minute presentation script + judge Q&A |

## 2. Setup in VS Code (one time, ~5 minutes)

1. Install **Python 3.9 or newer** from python.org (tick *"Add Python to PATH"*).
2. In VS Code: **File → Open Folder →** select the `AMR_Guard` folder.
3. Open the terminal: **View → Terminal** (or `` Ctrl + ` ``).
4. (Recommended) create a virtual environment:
   ```bash
   python -m venv venv
   # Windows:
   venv\Scripts\activate
   # Mac/Linux:
   source venv/bin/activate
   ```
5. Install the libraries:
   ```bash
   pip install -r requirements.txt
   ```

## 3. Run it

```bash
python generate_data.py     # step 1: make the dataset   (optional - app does it automatically)
python train_model.py       # step 2: train the AI        (optional - app does it automatically)
streamlit run app.py        # step 3: open the web app
```
A browser tab opens at **http://localhost:8501**. Stop it with `Ctrl + C` in the terminal.

Backup (no browser): `python demo_cli.py`

**Tip:** Run all three commands at home the day before, so presentation day starts instantly.
The app works fully **offline** once libraries are installed.

## 4. Two demo patients that tell the story

| | Patient A | Patient B |
|---|---|---|
| Organism / specimen | E. coli / Urine | Klebsiella pneumoniae / Blood |
| Setting | Community (OPD) | ICU |
| Age / gender | 28, Female | 70, Male |
| Risk factors | none | all four ticked |
| **Result** | ✅ Nitrofurantoin (Access) ~92% | 🚨 No safe option — refer to ID specialist |

Patient A shows **stewardship**: meropenem would also work, but AMR-Guard picks the simpler
Access drug. Patient B shows the **danger of AMR**: even last-resort colistin is uncertain.

## 5. Common errors

| Error | Fix |
|---|---|
| `'streamlit' is not recognized` | Use `python -m streamlit run app.py` |
| `ModuleNotFoundError` | Virtual env not active, or run `pip install -r requirements.txt` again |
| Error loading `models/amr_bundle.joblib` | Delete the `models` folder and run `python train_model.py` |
| `python` not found (Windows) | Try `py` instead of `python` |

## 6. Swapping in real data later
Replace `data/amr_isolates.csv` with a real antibiogram dataset that uses the same column
names (1 = Resistant, 0 = Susceptible, blank = not tested), delete `models/`, and retrain.
