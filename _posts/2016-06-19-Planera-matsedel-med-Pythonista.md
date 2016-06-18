---
layout: post
date: 2016-06-18
author: Jesper Svensson
title: Planera matsedel med Pythonista
---

Jag och min sambo brukar planera en veckomatsedel varje helg för kommande vecka. Samtidigt försöker vi skriva ner en inköpslista för att kunna följa planeringen. Detta har vi fram till nu gjort manuellt och hela tiden har vi fått sätta oss ner och gå igenom varje maträtt och lägga till alla ingredienser manuellt.

Detta är så otroligt tråkigt och tidskrävande så jag gjorde ett litet python-skript i Pythonista för att automatisera hela processen, dels välja maträtter och sedan automatiskt generera en inköpslista och skicka den till Påminnelser. Givetvis är det lite pill med att hela tiden lägga till nya maträtter och ingredienser men så småningom har vi ett helt bibliotek av rätter och då underlättar detta mycket.

  import dialogs
  import console
  import reminders
  import webbrowser

  # TODO: kontrollera så inte varan redan finns i Påminnelser
  # TODO: skapa en anteckning med planering i Drafts
  # TODO: lägg till lista med basvaror

  dishes = {1: ['köttfärs', 'tomatkross', 'lök', 'pasta'],
          2: ['köttfärs', 'majs', 'tomat', 'gurka', 'lök', 'tacosås', 'tacokrydda', 'tacobröd', 'tacochips'],
          3: ['ägg', 'mjölk', 'mjöl', 'steksmör', 'sylt'],
          4: ['falukorv', 'pasta'],
          5: ['kassler', 'potatis'],
          6: ['bacon', 'matgrädde', 'tomatkross', 'lök', 'vitlök', 'pasta', 'frysta grönsaker'],
          7: ['kyckling', 'wokgrönsaker', 'nudlar', 'thai sweet chili', 'soya'],
          8: ['köttfärs', 'vita bönor', 'kidneybönor', 'paprika', 'tomatkross', 'ris'],
          9: ['köttfärs', 'lasagnette-mix'],
          10: ['prinskorv', 'pasta'],
          11: ['köttfärs', 'ägg', 'ströbröd', 'potatis', 'matgrädde'],
          12: ['tonfisk', 'gurka', 'tomat', 'sallad', 'kikärtor', 'kidneybönor', 'salladsdressing']}

  names = ['Köttfärssås med pasta',
         'Tacos',
         'Pannkakor',
         'Falukorv med pasta',
         'Kassler med potatis',
         'Pasta med baconsås',
         'Kycklingwok',
         'Chili con carne',
         'Lasagnette',
         'Prinskorv med pasta',
         'Pannbiff med potatis',
         'Tonfisksallad']

  shopping_list = []

  # lägger till inköpslista i Påminnelser
  def add_to_reminder(s_list):
  all_calendars = reminders.get_all_calendars()
  for calendar in all_calendars:
    if calendar.title == 'Mat':
      for i in s_list:
        r = reminders.Reminder(calendar)
        r.title = i
        r.save()
  #console.alert('Klart','Alla varor har lagts till i Påminnelser...','OK')
  #webbrowser.open('x-apple-reminder://')
  print('\n')
  console.write_link('Gå till Påminnelser', 'x-apple-reminder://')
  print('\n')

  # redigerar och skapar den slutgiltiga inköpslistan
  def create_shopping_list():
  print('\n\nInköpslista:\n')
  mod_shopping_list = dialogs.edit_list_dialog('Lista', shopping_list)
  for i in mod_shopping_list:
    print(i)
  add_to_reminder(mod_shopping_list)

  # lägger till varor i inköpslistan
  def add_ingrediens(l):
  for i in l:
    if i in shopping_list:
      pass
    else:
      shopping_list.append(i)

  # skriver rätter i konsolen
  def print_names():
  x = 0
  while x < len(names):
    print(str(x + 1) + '. ' + names[x])
    x = x + 1
  choose()

  # lägger till alla ingredienser från de valda rätterna
  def add_to_shopping_list(chosen_dishes):
  for i in chosen_dishes:
    add_ingrediens(dishes[int(i)])
  create_shopping_list()

  # låter användare välja rätter, val separeras med ett mellanslag
  def choose():
  chosen_dishes = []
  choice = input('\nVälj rätter: ')
  chosen_dishes = choice.split()
  add_to_shopping_list(chosen_dishes)

  # startar skriptet
  def main():
  console.clear()
  console.set_font('Avenir', 20)
  print_names()

  if __name__ == '__main__':
  main()
