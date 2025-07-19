# 🏥 Patient Data Validator using Pydantic

This project demonstrates how to use **Pydantic**'s `BaseModel` and custom validators to enforce rules and transform patient data in Python.

## ✅ Features

- Strict data validation using `Pydantic`
- Custom validators using `@field_validator` for:
  - Ensuring email domain validity
  - Auto-transforming name to uppercase
  - Restricting age within a valid range
- Type coercion for automatic casting (e.g., string `"30"` to int)

## 📋 Patient Model Fields

```python
name: str (auto converted to UPPERCASE)
email: EmailStr (must be from hdfc.com or icici.com)
age: int (0 < age < 100)
weight: float
married: bool
allergies: List[str]
contact_details: Dict[str, str]
