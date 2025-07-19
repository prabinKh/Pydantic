# 🏥 Patient and Address Data with Nested Pydantic Models

This Python project demonstrates how to use **Pydantic** for nested model validation by creating a `Patient` model that includes an embedded `Address` model.

---

## ✅ Features

- Structured data validation using Pydantic
- Demonstrates nested models (`Patient` contains `Address`)
- Showcases Pydantic's `.model_dump()` method for exporting data
- Clean and Pythonic use of type annotations

---

## 📦 Models Overview

### 📍 Address Model

```python
class Address(BaseModel):
    city: str
    state: str
    pin: str
