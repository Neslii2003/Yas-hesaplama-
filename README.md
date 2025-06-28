import datetime
import tkinter as tk
import os  
from PIL import Image, ImageTk

window = tk.Tk()
window.geometry("620x780")
window.title("Age Calculator App")

icon_path = os.path.join(os.path.dirname(__file__), "age_calculator_icon_v2.ico")
window.iconbitmap(icon_path)

tk.Label(window, text="Name").grid(column=0, row=1)
tk.Label(window, text="Year").grid(column=0, row=2)
tk.Label(window, text="Month").grid(column=0, row=3)
tk.Label(window, text="Day").grid(column=0, row=4)

nameEntry = tk.Entry(window)
nameEntry.grid(column=1, row=1)

yearEntry = tk.Entry(window)
yearEntry.grid(column=1, row=2)

monthEntry = tk.Entry(window)
monthEntry.grid(column=1, row=3)

dateEntry = tk.Entry(window)
dateEntry.grid(column=1, row=4)

textArea = tk.Text(master=window, height=7, width=45)
textArea.grid(column=1, row=6)


class Person:
    def __init__(self, name, birthdate):
        self.name = name
        self.birthdate = birthdate

    def age(self):
        today = datetime.date.today()
        age = today.year - self.birthdate.year
        if (today.month, today.day) < (self.birthdate.month, self.birthdate.day):
            age -= 1
        return age


def get_zodiac_sign(day, month):
    if (month == 1 and day >= 20) or (month == 2 and day <= 18):
        return "Kova"
    elif (month == 2 and day >= 19) or (month == 3 and day <= 20):
        return "Balık"
    elif (month == 3 and day >= 21) or (month == 4 and day <= 19):
        return "Koç"
    elif (month == 4 and day >= 20) or (month == 5 and day <= 20):
        return "Boğa"
    elif (month == 5 and day >= 21) or (month == 6 and day <= 20):
        return "İkizler"
    elif (month == 6 and day >= 21) or (month == 7 and day <= 22):
        return "Yengeç"
    elif (month == 7 and day >= 23) or (month == 8 and day <= 22):
        return "Aslan"
    elif (month == 8 and day >= 23) or (month == 9 and day <= 22):
        return "Başak"
    elif (month == 9 and day >= 23) or (month == 10 and day <= 22):
        return "Terazi"
    elif (month == 10 and day >= 23) or (month == 11 and day <= 21):
        return "Akrep"
    elif (month == 11 and day >= 22) or (month == 12 and day <= 21):
        return "Yay"
    else:
        return "Oğlak"


zodiac_comments = {
    "Koç": "Bugün enerjin yüksek! Yeni başlangıçlar için harika bir gün.",
    "Boğa": "Sabırlı ol, güzel haberler yolda. Finansal konularda şanslısın.",
    "İkizler": "Sosyallik ön planda, arkadaşlarınla vakit geçir.",
    "Yengeç": "Duygusal yoğunluk yaşayabilirsin. Aileyle iletişim güçleniyor.",
    "Aslan": "Kendine güven! Liderlik özelliklerin parlıyor.",
    "Başak": "Detaylara dikkat et, başarı seni bulacak.",
    "Terazi": "Dengeyi kurmak zor olabilir ama senin doğanda bu var.",
    "Akrep": "Gizemli havasınla dikkat çekiyorsun. Hislerine güven.",
    "Yay": "Macera seni çağırıyor! Yeni şeyler denemekten çekinme.",
    "Oğlak": "Disiplinli çalışman ödüllendirilecek.",
    "Kova": "Yenilikçi fikirlerinle çevreni etkiliyorsun.",
    "Balık": "Hayal gücün bugün çok güçlü. Sanatsal faaliyetler için ideal."
}


def getInput():
    name = nameEntry.get()
    try:
        year = int(yearEntry.get())
        month = int(monthEntry.get())
        day = int(dateEntry.get())
        birthdate = datetime.date(year, month, day)
        person = Person(name, birthdate)
        zodiac = get_zodiac_sign(day, month)
        comment = zodiac_comments.get(zodiac, "Bugün güzel bir gün olabilir :)")
        
        result = f"Heyy {person.name}!!!\n"
        result += f"You are {person.age()} years old!\n"
        result += f"Your zodiac sign is: {zodiac} ♈\n"
        result += f"Horoscope: {comment}"
    except ValueError:
        result = "Lütfen geçerli bir tarih giriniz!"

    textArea.delete("1.0", tk.END)
    textArea.insert(tk.END, result)


tk.Button(window, text="Calculate Age", command=getInput, bg="pink").grid(column=1, row=5)

image = Image.open("age_calculator_icon_v2.ico")
image = image.resize((300, 300))
photo = ImageTk.PhotoImage(image)
tk.Label(window, image=photo).grid(column=1, row=0)

window.mainloop()
