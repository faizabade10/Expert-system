# Expert-system
import tkinter as tk
from tkinter import messagebox

# Prior probabilities
P_PASS = 0.60
P_FAIL = 0.40

# Conditional probabilities
# (Probability when Pass, Probability when Fail)

probabilities = {
    "study": (0.80, 0.30),
    "attendance": (0.85, 0.40),
    "marks": (0.80, 0.25)
}


def predict_performance():

    study = study_var.get()
    attendance = attendance_var.get()
    marks = marks_var.get()

    if study == "" or attendance == "" or marks == "":
        messagebox.showwarning(
            "Missing Information",
            "Please select all student details."
        )
        return

    evidence = {
        "study": study == "High",
        "attendance": attendance == "High",
        "marks": marks == "Good"
    }

    likelihood_pass = 1.0
    likelihood_fail = 1.0

    for variable, value in evidence.items():

        p_pass, p_fail = probabilities[variable]

        if value:
            likelihood_pass *= p_pass
            likelihood_fail *= p_fail
        else:
            likelihood_pass *= (1 - p_pass)
            likelihood_fail *= (1 - p_fail)

    numerator = likelihood_pass * P_PASS

    denominator = (
        likelihood_pass * P_PASS
        + likelihood_fail * P_FAIL
    )

    posterior = numerator / denominator

    probability_label.config(
        text=f"Probability of Passing: {posterior * 100:.2f}%"
    )

    if posterior >= 0.50:
        result_label.config(
            text="Prediction: Student is likely to PASS."
        )
    else:
        result_label.config(
            text="Prediction: Student is likely to FAIL."
        )


def clear_data():

    study_var.set("")
    attendance_var.set("")
    marks_var.set("")

    probability_label.config(
        text="Probability of Passing: --"
    )

    result_label.config(
        text="Prediction: --"
    )


# Main Window
root = tk.Tk()
root.title("Student Exam Performance Prediction")
root.geometry("600x550")

# Heading
title = tk.Label(
    root,
    text="Student Exam Performance Prediction",
    font=("Arial", 18, "bold")
)
title.pack(pady=20)

subtitle = tk.Label(
    root,
    text="Bayesian Belief Network Expert System",
    font=("Arial", 11)
)
subtitle.pack()

# Variables
study_var = tk.StringVar()
attendance_var = tk.StringVar()
marks_var = tk.StringVar()

# Study Hours
tk.Label(
    root,
    text="Study Hours:",
    font=("Arial", 12)
).pack(pady=(25, 5))

study_menu = tk.OptionMenu(
    root,
    study_var,
    "High",
    "Low"
)
study_menu.pack()

# Attendance
tk.Label(
    root,
    text="Attendance:",
    font=("Arial", 12)
).pack(pady=(15, 5))

attendance_menu = tk.OptionMenu(
    root,
    attendance_var,
    "High",
    "Low"
)
attendance_menu.pack()

# Previous Marks
tk.Label(
    root,
    text="Previous Marks:",
    font=("Arial", 12)
).pack(pady=(15, 5))

marks_menu = tk.OptionMenu(
    root,
    marks_var,
    "Good",
    "Poor"
)
marks_menu.pack()

# Predict Button
predict_button = tk.Button(
    root,
    text="Predict Performance",
    command=predict_performance,
    font=("Arial", 12)
)
predict_button.pack(pady=25)

# Probability
probability_label = tk.Label(
    root,
    text="Probability of Passing: --",
    font=("Arial", 13, "bold")
)
probability_label.pack(pady=10)

# Result
result_label = tk.Label(
    root,
    text="Prediction: --",
    font=("Arial", 12)
)
result_label.pack(pady=10)

# Clear Button
clear_button = tk.Button(
    root,
    text="Clear",
    command=clear_data,
    font=("Arial", 11)
)
clear_button.pack(pady=15)

root.mainloop()
