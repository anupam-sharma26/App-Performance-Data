# App-Performance-Data
This dashboard provides an overview of app performance, tracking user growth, engagement, and monetization metrics. It highlights DAU, WAU, signups, logins, ad activity, coupon requests, and retention trends, along with monthly active users and key user journey insights.


-- USER METRICS
DAU = DISTINCTCOUNT('Events'[user_id])

WAU =
CALCULATE(
    DISTINCTCOUNT('Events'[user_id]),
    DATESINPERIOD('Date'[Date], MAX('Date'[Date]), -7, DAY)
)

MAU =
CALCULATE(
    DISTINCTCOUNT('Events'[user_id]),
    DATESINPERIOD('Date'[Date], MAX('Date'[Date]), -30, DAY)
)

-- NEW USERS / INSTALLS
New Users =
CALCULATE(
    DISTINCTCOUNT('Users'[user_id]),
    'Users'[is_new_user] = TRUE()
)

Installs = COUNT('Users'[user_id])

-- EVENT METRICS
Signups (Success) =
CALCULATE(
    COUNT('Events'[event_name]),
    'Events'[event_name] = "signup_success"
)

Logins (Success) =
CALCULATE(
    COUNT('Events'[event_name]),
    'Events'[event_name] = "login_success"
)

Ads Opened =
CALCULATE(
    COUNT('Events'[event_name]),
    'Events'[event_name] = "ad_open"
)

Ads Closed =
CALCULATE(
    COUNT('Events'[event_name]),
    'Events'[event_name] = "ad_close"
)

Coupon Requested =
CALCULATE(
    COUNT('Events'[event_name]),
    'Events'[event_name] = "coupon_request"
)

-- ENGAGEMENT METRICS
Screen Views =
CALCULATE(
    COUNT('Events'[event_name]),
    'Events'[event_name] = "screen_view"
)

Page Visits =
CALCULATE(
    COUNT('Events'[event_name]),
    'Events'[event_name] = "page_visit"
)

Engaged Sessions =
CALCULATE(
    COUNT('Events'[session_id]),
    'Events'[engaged_session] = TRUE()
)

-- RETENTION
Returning Users =
CALCULATE(
    DISTINCTCOUNT('Events'[user_id]),
    'Events'[is_returning_user] = TRUE()
)

Total Users = DISTINCTCOUNT('Events'[user_id])

Retention % =
DIVIDE(
    [Returning Users],
    [Total Users],
    0
)

-- MONTHLY TREND
Monthly Active Users =
CALCULATE(
    DISTINCTCOUNT('Events'[user_id]),
    ALLEXCEPT('Date', 'Date'[Month], 'Date'[Year])
)
