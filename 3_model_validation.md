# 🧬 Patient Data Validator with Pydantic

This project demonstrates the use of [**Pydantic**](https://docs.pydantic.dev) for modeling and validating patient data using Python.

It leverages field-level validation and **model-level custom logic** using `@model_validator`.

---

## 🚀 Features

- Strongly typed schema with auto-type coercion
- Uses Pydantic's `BaseModel` for data validation
- Implements custom rule:
  > Patients over age 60 must have an emergency contact in `contact_details`
- Easy-to-read code with structured output

---

## 📦 Patient Model Schema

```python
class Patient(BaseModel):
    name: str
    email: EmailStr
    age: int
    weight: float
    married: bool
    allergies: List[str]
    contact_details: Dict[str, str]
