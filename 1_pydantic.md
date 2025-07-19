# Patient Data Management with Pydantic

This Python module demonstrates how to use **Pydantic** for data validation and structured data representation using the `Patient` model.

## Features

- Validates patient information using `pydantic.BaseModel`
- Uses `Annotated`, `Field`, and built-in types for strict validation
- Enforces constraints like:
  - Max length of name
  - Valid email and URL
  - Age and weight boundaries
  - Optional list of allergies (max 5)
- Accepts nested contact details via dictionary

## Example Fields

```python
name: str
email: EmailStr
linkedin_url: AnyUrl
age: int (0 < age < 120)
weight: float (strict, > 0)
married: Optional[bool]
allergies: Optional[List[str]] (max 5)
contact_details: Dict[str, str]
