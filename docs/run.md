### Run code guide

- Installed `64-Bit (Apple silicon) Graphical Installer`
- Verify Installation `conda --version`
- Initialize Conda `conda init` Then close Terminal completely and open it again.
- Create a Clean Environment `conda create -n parkinsons python=3.11`
- Activate Your Environment `conda activate parkinsons`
- Install Jupyter `conda install jupyter`
- Install & register ipykernel `python -m ipykernel install --user --name pd_ai --display-name "Python (pd_ai)"`
- Launch Jupyter Notebook `jupyter notebook`
- In Jupyter - Kernel selector will show `Python (pd_ai)`