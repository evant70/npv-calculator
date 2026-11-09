
import math
import streamlit as st
import numpy_financial as npf
import pandas as pd

# Set up the application
st.set_page_config(page_title="Finance Calculator", page_icon="📊")

st.title("📊 NPV & IRR Calculator")
st.write("Calculate the value and return of an investment.")

# Allow the user to choose the number of years
years = st.slider("Number of years", 1, 10, 3)

# Create the input form
with st.form("calculator"):

    investment = st.number_input(
        "Initial Investment ($)",
        min_value=0.0,
        value=10000.0
    )

    rate = st.number_input(
        "Discount Rate (%)",
        min_value=0.0,
        value=10.0
    )

    st.subheader("Annual Cash Flows")

    cash_flows = []

    for year in range(1, years + 1):
        cash_flow = st.number_input(
            f"Year {year} Cash Flow ($)",
            value=4000.0,
            key=f"year_{year}"
        )
        cash_flows.append(cash_flow)

    # Create the calculation button
    calculate = st.form_submit_button("Calculate")

# Perform calculations when the button is clicked
if calculate:

    # Convert percentage to decimal
    discount_rate = rate / 100

    # Calculate Net Present Value
    npv = -investment

    for year, cash_flow in enumerate(cash_flows, start=1):
        npv += cash_flow / (1 + discount_rate) ** year

    # Calculate Internal Rate of Return
    all_flows = [-investment] + cash_flows
    irr = npf.irr(all_flows)

    # Display the results
    st.subheader("Investment Results")

    col1, col2 = st.columns(2)

    col1.metric("NPV", f"${npv:,.2f}")

    if math.isfinite(irr):
        col2.metric("IRR", f"{irr * 100:.2f}%")
    else:
        col2.metric("IRR", "Not available")

    # Explain the investment decision
    if npv > 0:
        st.success("Positive NPV: The investment creates value.")
    elif npv < 0:
        st.error("Negative NPV: The investment destroys value.")
    else:
        st.info("NPV is zero: The investment breaks even.")

    # Display the NPV formula
    st.subheader("Formula Used")

    st.latex(
        r"NPV = -I_0 + \sum_{t=1}^{n} \frac{CF_t}{(1+r)^t}"
    )

    # Show each year's calculation
    st.subheader("Cash Flow Breakdown")

    data = []

    for year, cash_flow in enumerate(cash_flows, start=1):
        present_value = cash_flow / (1 + discount_rate) ** year

        data.append({
            "Year": year,
            "Cash Flow": cash_flow,
            "Present Value": present_value
        })

    df = pd.DataFrame(data)

    st.dataframe(df, hide_index=True)

    # Create a visual chart
    st.subheader("Present Value by Year")
    st.bar_chart(df.set_index("Year")["Present Value"])
