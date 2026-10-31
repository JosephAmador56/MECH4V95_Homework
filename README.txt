Joseph Amador

Install all dependencies and libraries listed in requirements.txt
Python version: 3.14

All random seeds were set to SEED = 2026

There are 2 notebooks to run:
1. homework_notebook (Exact GP)
    Produces the outputs for B1. The only changes necessary to reproduce each result is to set: subset = "n samples
     to use", alpha = "the specified multiplier found in the results table for each section", and restarts = "the
     number given in each results table title"
    Uncomment the save command in line 33 to write new csv files
2. homework_notebook_B2 (Scalable GP)
    Produces the outputs for B2. The only changes necessary to reproduce each result is to set: num_inducing_points =
     "number of inducing points"
    The noise for B2 was pulled from the status bar's final noise value that shows when training the model
    Note: since I used my Nvidia GPU to train the scalable GP model it might not work on your system.

Swap the following to S1 or S3 in each notebook to change the testing set
idx = S3["train"]

# Get X: Input feature arrays and Y: Target arrays in ln(CHF)
X_val, Y_val = get_xy(S3["val"])
X_test, Y_test = get_xy(S3["test"])
X_train,Y_train = get_xy(S3["train"])

save_chf_predictions(f"B4-{subset}", S3["test"], np.exp(S1_Test_mean_prediction), lower = metrics["Lower"], upper = metrics["Upper"], split_name = "S3Test")

# Tuning Outputs
diagnostic_plots(idx, mean_prediction, f"GP ({subset} samples)")
for name, j in [("S1 val", S1["val"]), ("S1 test", S1["test"]), ("low-G test", LOWG_TEST)]:
    record(f"GP ({subset} samples, alpha {alpha:.5f}, restarts {restarts})", name, df[TARGET].to_numpy()[j], np.exp(gp.predict(scaler.transform(get_xy(j)[0]))), **{"Training Time": end_time - start_time, "RBF (P, G, D, X)": gp.kernel_, "PICP_95": metrics["PICP_95"], "MRIW": metrics["MRIW"], "NLL_log": metrics["NLL_log"]})


The CustomGraphs.xlsx file was used to generate the training time, calibration curve, predicted σ against absolute error
, and mass flux vs. stddev figures