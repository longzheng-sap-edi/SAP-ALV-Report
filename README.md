
SAP Simple ABAB ALV report
---

## **Step 1 — Prepare the data structures**
Every ALV report begins with defining the structures and internal tables that will hold the data.
We create a custom type for the material information, and then build an internal table based on that type.

---

## **Step 2 — Create the selection screen**
Next, we provide a simple selection screen for the user.
In this example, the user can filter materials by material number or material group.
This makes the report flexible and user-friendly.

---

## **Step 3 — Read the data from the database**
Then we read the data from MARA and MAKT using a SELECT statement.
After loading the data, we also add a row number so the ALV can show a sequence.

---

## **Step 4 — Build the field catalog**
The field catalog controls how each field appears in the ALV:
its label, alignment, and display rules.
We prepare the catalog once and then pass it to the ALV function.

---

## **Step 5 — Set the ALV layout**

Next, we define the ALV layout.

Here we enable:

* checkboxes
* optimized column width
* zebra pattern
* and general display settings

These options improve the UI and usability.

---

## **Step 6 — Display the ALV**

Finally, we call the standard ALV function module to display the table.

Once the layout, field catalog, and internal table are passed in, SAP generates the grid automatically.

---

## **Closing**

And that’s the complete structure of creating an ALV report — simple, clear, and following SAP best practices.

If you like this type of content, feel free to subscribe, and I’ll create more SAP tutorials in future videos.

Thanks for watching!
---

