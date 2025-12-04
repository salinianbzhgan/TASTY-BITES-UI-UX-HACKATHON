import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime

# -----------------------------------------------
# PAGE CONFIGURATION
# -----------------------------------------------
st.set_page_config(page_title="Tasty Bites 🍴", layout="wide")

# -----------------------------------------------
# LOGIN SYSTEM
# -----------------------------------------------
users = {
    "pooja": "pass123",
    "guest": "guest"
}

if "logged_in" not in st.session_state:
    st.session_state.logged_in = False
if "username" not in st.session_state:
    st.session_state.username = ""
if "goal" not in st.session_state:
    st.session_state.goal = 2000  # default calorie goal

# Login Page
if not st.session_state.logged_in:
    st.markdown("<h1 style='text-align:center; color:#ff7043;'>🍴 Welcome to Tasty Bites 🍴</h1>", unsafe_allow_html=True)
    st.markdown("<p style='text-align:center; color:gray;'>Smart Food & Health Tracker</p>", unsafe_allow_html=True)
    with st.form("login_form"):
        st.write("### 🔐 Login to Continue")
        username = st.text_input("Username")
        password = st.text_input("Password", type="password")
        submitted = st.form_submit_button("Login")
        if submitted:
            if username in users and users[username] == password:
                st.session_state.logged_in = True
                st.session_state.username = username
                st.success(f"Welcome, {username}! 🎉")
                st.experimental_rerun()
            else:
                st.error("Invalid username or password ❌")
    st.stop()

# -----------------------------------------------
# RECIPE DATABASE WITH SIMPLE INSTRUCTIONS
# -----------------------------------------------
recipes = {
    "Breakfast": [
        {"name": "Idli with Sambar", "ingredients": {"Idli Batter": "2 cups", "Sambar": "1 cup"},
         "serves": 2, "calories": 350,
         "steps": "Steam idlis using batter for 10 minutes. Serve hot with sambar and chutney."},

        {"name": "Oats Upma", "ingredients": {"Oats": "1 cup", "Vegetables": "1 cup"},
         "serves": 2, "calories": 280,
         "steps": "Roast oats, sauté veggies with spices, mix and cook for 5 minutes."},

        {"name": "Egg Toast", "ingredients": {"Bread": "2 slices", "Egg": "2"},
         "serves": 1, "calories": 300,
         "steps": "Beat eggs, dip bread slices, and toast both sides until golden brown."},

        {"name": "Smoothie Bowl", "ingredients": {"Banana": "1", "Milk": "1 cup", "Nuts": "2 tbsp"},
         "serves": 1, "calories": 400,
         "steps": "Blend banana and milk, pour into a bowl, and top with chopped nuts."},

        {"name": "Poha", "ingredients": {"Flattened rice": "1 cup", "Onion": "1", "Lemon": "1"},
         "serves": 2, "calories": 320,
         "steps": "Rinse poha, sauté onion and spices, mix poha, cook for 3 minutes, and squeeze lemon."},
    ],

    "Lunch": [
        {"name": "Veg Biryani", "ingredients": {"Rice": "2 cups", "Vegetables": "2 cups"},
         "serves": 3, "calories": 600,
         "steps": "Cook rice and veggies with spices. Layer and steam for rich aroma."},

        {"name": "Paneer Butter Masala", "ingredients": {"Paneer": "150g", "Tomato": "2"},
         "serves": 2, "calories": 550,
         "steps": "Cook tomato gravy, add paneer cubes, butter, and simmer for 5 minutes."},

        {"name": "Chicken Curry with Rice", "ingredients": {"Chicken": "200g", "Rice": "1 cup"},
         "serves": 2, "calories": 700,
         "steps": "Cook chicken in spiced curry base. Serve hot with steamed rice."},

        {"name": "Curd Rice", "ingredients": {"Curd": "1 cup", "Rice": "1 cup"},
         "serves": 1, "calories": 350,
         "steps": "Mix rice and curd, season with mustard seeds, and serve chilled."},

        {"name": "Sambar Rice", "ingredients": {"Rice": "1 cup", "Sambar": "1 cup"},
         "serves": 1, "calories": 380,
         "steps": "Mix cooked rice with sambar, drizzle ghee, and serve hot."},

        {"name": "Veg Fried Rice", "ingredients": {"Rice": "2 cups", "Soy Sauce": "2 tsp"},
         "serves": 2, "calories": 450,
         "steps": "Stir-fry rice with veggies and soy sauce for 5 minutes."},

        {"name": "Fish Curry with Rice", "ingredients": {"Fish": "150g", "Rice": "1 cup"},
         "serves": 2, "calories": 620,
         "steps": "Cook fish in coconut-based curry and serve with rice."},

        {"name": "Chapathi with Kurma", "ingredients": {"Chapathi": "2", "Kurma": "1 cup"},
         "serves": 2, "calories": 400,
         "steps": "Cook mixed vegetable kurma and serve warm with chapathi."},

        {"name": "Pulao", "ingredients": {"Rice": "2 cups", "Vegetables": "2 cups"},
         "serves": 2, "calories": 480,
         "steps": "Cook rice with spices, sautéed veggies, and ghee."},

        {"name": "Grilled Chicken Salad", "ingredients": {"Chicken": "150g", "Lettuce": "1 cup"},
         "serves": 1, "calories": 320,
         "steps": "Grill chicken, toss with lettuce, olive oil, and lemon juice."},
    ],

    "Desserts": [
        {"name": "Fruit Salad", "ingredients": {"Fruits": "1 cup", "Honey": "1 tbsp"},
         "serves": 2, "calories": 250,
         "steps": "Chop fruits, drizzle honey, and refrigerate before serving."},

        {"name": "Gulab Jamun", "ingredients": {"Jamuns": "3", "Syrup": "1/2 cup"},
         "serves": 1, "calories": 400,
         "steps": "Fry jamuns and soak in warm sugar syrup for 10 minutes."},

        {"name": "Ice Cream", "ingredients": {"Milk": "1 cup", "Sugar": "2 tbsp"},
         "serves": 1, "calories": 380,
         "steps": "Mix milk, sugar, freeze, and churn for creamy texture."},

        {"name": "Chocolate Mousse", "ingredients": {"Chocolate": "100g", "Cream": "1/2 cup"},
         "serves": 2, "calories": 420,
         "steps": "Melt chocolate, fold in whipped cream, and chill."},
    ],

    "Drinks": [
        {"name": "Lassi", "ingredients": {"Curd": "1 cup", "Sugar": "1 tbsp"},
         "serves": 1, "calories": 180,
         "steps": "Blend curd, sugar, and water until frothy."},

        {"name": "Green Smoothie", "ingredients": {"Spinach": "1 cup", "Banana": "1"},
         "serves": 1, "calories": 200,
         "steps": "Blend spinach and banana for a refreshing smoothie."}
    ]
}

# -----------------------------------------------
# SIDEBAR NAVIGATION
# -----------------------------------------------
st.sidebar.title(f"👩‍🍳 Welcome, {st.session_state.username}")
page = st.sidebar.radio("Navigate", ["🏠 Home", "🍲 Recipes", "🛒 Groceries", "📊 Calorie Chart", "💪 Health Tracker", "🚪 Logout"])

# -----------------------------------------------
# HOME PAGE
# -----------------------------------------------
if page == "🏠 Home":
    st.markdown("<h1 style='color:#ff7043;'>🍽️ Tasty Bites</h1>", unsafe_allow_html=True)
    st.image("https://i.ibb.co/6sRZnWD/healthy-food-banner.jpg", use_container_width=True)
    st.info("Track your meals, monitor your calorie intake, and explore healthy recipes with Tasty Bites!")

# -----------------------------------------------
# RECIPES PAGE
# -----------------------------------------------
elif page == "🍲 Recipes":
    st.subheader("🍛 Explore Recipes by Category")
    category = st.selectbox("Choose a category", list(recipes.keys()))
    for r in recipes[category]:
        with st.expander(f"🍴 {r['name']}"):
            st.write(f"**Ingredients:** {', '.join(r['ingredients'].keys())}")
            st.write(f"**Serves:** {r['serves']}")
            st.write(f"**Calories:** {r['calories']} kcal")
            st.write(f"**Preparation:** {r['steps']}")
            if st.button(f"Add {r['name']} to My Calorie Tracker", key=r['name']):
                today = datetime.now().date()
                if "log" not in st.session_state:
                    st.session_state.log = []
                st.session_state.log.append({"date": today, "item": r["name"], "calories": r["calories"]})
                st.success(f"{r['name']} added to your calorie tracker! ✅")

# -----------------------------------------------
# GROCERIES PAGE
# -----------------------------------------------
elif page == "🛒 Groceries":
    st.subheader("🧺 Your Groceries Inbox")
    user_input = st.text_area("Enter available ingredients (comma-separated):")
    if st.button("Find Recipes 🍳"):
        suggestions = []
        for cat, recipe_list in recipes.items():
            for r in recipe_list:
                if any(i.lower() in user_input.lower() for i in r["ingredients"].keys()):
                    suggestions.append(r["name"])
        if suggestions:
            st.success("You can make these with your available ingredients:")
            for s in suggestions:
                st.write(f"- 🥘 {s}")
        else:
            st.warning("No recipes match your groceries. Try adding more ingredients!")

# -----------------------------------------------
# CALORIE CHART PAGE
# -----------------------------------------------
elif page == "📊 Calorie Chart":
    st.subheader("📅 Your Calorie Progress")
    goal = st.number_input("🎯 Set Your Daily Calorie Goal", value=st.session_state.goal, step=100)
    st.session_state.goal = goal

    if "log" not in st.session_state or len(st.session_state.log) == 0:
        st.info("No meals logged yet! Go to Recipes and add some 🍲")
    else:
        df = pd.DataFrame(st.session_state.log)
        daily = df.groupby("date")["calories"].sum().reset_index()

        fig, ax = plt.subplots(figsize=(10, 5))
        colors = ["green" if c <= goal else "red" for c in daily["calories"]]
        ax.bar(daily["date"], daily["calories"], color=colors, alpha=0.8)
        ax.axhline(y=goal, color="blue", linestyle="--", label="Calorie Goal")
        ax.plot(daily["date"], daily["calories"], color="orange", marker="o", linewidth=2)
        ax.set_title("📊 Daily Calorie Intake vs Goal", fontsize=15)
        ax.set_xlabel("Date")
        ax.set_ylabel("Calories (kcal)")
        ax.legend()
        st.pyplot(fig)

# -----------------------------------------------
# HEALTH TRACKER PAGE
# -----------------------------------------------
elif page == "💪 Health Tracker":
    st.subheader("🏃‍♀️ Calculate Your Health Measures")
    height = st.number_input("Height (cm):", min_value=100.0, max_value=250.0, step=0.5)
    weight = st.number_input("Weight (kg):", min_value=30.0, max_value=200.0, step=0.5)
    age = st.number_input("Age:", min_value=10, max_value=100)
    gender = st.selectbox("Gender", ["Male", "Female"])

    if st.button("Calculate"):
        bmi = weight / ((height / 100) ** 2)
        if gender == "Male":
            bmr = 88.36 + (13.4 * weight) + (4.8 * height) - (5.7 * age)
        else:
            bmr = 447.6 + (9.2 * weight) + (3.1 * height) - (4.3 * age)
        st.success(f"💪 BMI: {bmi:.2f} | BMR: {bmr:.2f} kcal/day")
        if bmi < 18.5:
            st.info("Underweight — include more calories in your diet.")
        elif bmi < 25:
            st.success("Healthy range — keep it up 🌟")
        elif bmi < 30:
            st.warning("Overweight — try balancing meals.")
        else:
            st.error("Obese range — consult a doctor for advice.")

# -----------------------------------------------
# LOGOUT PAGE
# -----------------------------------------------
elif page == "🚪 Logout":
    st.session_state.logged_in = False
    st.session_state.username = ""
    st.success("Logged out successfully!")
    st.experimental_rerun()
