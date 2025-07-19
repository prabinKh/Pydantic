# 🧑‍⚕️ Nested Patient Model with Default Values using Pydantic

This example demonstrates how to define nested data models in **Pydantic**, set default values, and serialize model data using `.model_dump()` with `exclude_unset=True`.

---

## ✅ Key Features

- Models patient information with a nested `Address` model
- Uses default values for fields (e.g., gender defaults to `'Male'`)
- Utilizes `.model_dump(exclude_unset=True)` to exclude default values from output
- Clean dictionary-based initialization and validation

---

## 📦 Model Overview

### 🏠 Address Model

```python
class Address(BaseModel):
    city: str
    state: str
    pin: str
