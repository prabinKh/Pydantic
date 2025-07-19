# 🧑‍⚕️ Patient Data with Computed BMI using Pydantic

This project demonstrates how to model and validate patient data in Python using **Pydantic**, while also computing the **BMI (Body Mass Index)** using the `@computed_field` decorator.

---

## ✅ Features

- Strong type enforcement with Pydantic's `BaseModel`
- Automatic calculation of BMI using a computed property
- Nested dictionary support for `contact_details`
- Clean and readable structure for healthcare-related data

---

## 📋 Patient Model Schema

```python
class Patient(BaseModel):
    name: str
    email: EmailStr
    age: int
    weight: float      # in kilograms
    height: float      # in meters
    married: bool
    allergies: List[str]
    contact_details: Dict[str, str]
