# Contributing Guide

Thank you for considering contributing to the Qidi X‑Plus 3 Macro Suite.  
This project aims to provide a clean, reliable, and maintainable set of Klipper macros for the Qidi ecosystem.

---

## 🧭 Contribution Principles

1. **Safety first**  
   All macros must be Qidi‑safe and avoid:
   - auto‑resume behavior  
   - double‑movement  
   - heater conflicts  
   - undefined variables  

2. **Consistency**  
   Follow the existing macro style:
   - snake_case variable names  
   - clear comments  
   - consistent indentation  
   - SMART_PARK for all parking moves  

3. **Minimal slicer requirements**  
   Macros should not require slicer hacks unless absolutely necessary.

4. **Predictability**  
   Avoid hidden behavior or implicit state changes.

---

## 🛠 How to Contribute

### 1. Fork the repository  
Create your own branch for changes.

### 2. Make your changes  
Ensure:
- macros validate in Klipper  
- no Qidi auto‑resume issues  
- no double‑restore behavior  

### 3. Add documentation  
If you add or change behavior, update:
- `docs/`  
- `CHANGELOG.md`  

### 4. Submit a Pull Request  
Include:
- what changed  
- why it changed  
- how it was tested  

---

## 🧪 Testing Checklist

Before submitting a PR, verify:

- [ ] PAUSE works from LCD and from G‑code  
- [ ] RESUME restores state cleanly  
- [ ] CANCEL_PRINT does not double‑execute PRINT_END  
- [ ] No double Z‑hop  
- [ ] No double prime  
- [ ] No unexpected motion  
- [ ] No undefined variables  
- [ ] No heater left on after cancel  

---

## 💬 Communication

Open an issue for:
- bug reports  
- feature requests  
- macro design discussions  