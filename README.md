# The Goal

The goal is to find where to place some fake computing loop / data fetching function to reproduce
the following:
 User is on page A
 User navigates to page B
 User clicks on the button
 The click action is not executed
 User clicks on the button a bit later
 The click action is executed

The first click needs to happen before the fake computing loop finishes.
