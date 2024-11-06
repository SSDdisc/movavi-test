import telebot

bot = telebot.TeleBot('')

seats = {
    '1A': False,
    '1B': False,
    '1C': False,
    '2A': False,
    '2B': False,
    '2C': False,
}

@bot.message_handler(commands=['start'])
def start(message):
    available_seats = [seat for seat, booked in seats.items() if not booked]
    response = "Доступные места: " + ", ".join(available_seats) if available_seats else "Нет доступных мест."
    bot.send_message(message.chat.id, response)

@bot.message_handler(func=lambda message: True)
def book_seat(message):
    seat = message.text.strip().upper() 
    if seat in seats: 
        if not seats[seat]: 
            seats[seat] = True 
            bot.send_message(message.chat.id, f"Место {seat} успешно забронировано!")  
        else:
            bot.send_message(message.chat.id, f"Место {seat} уже забронировано.")  
    else:
        bot.send_message(message.chat.id, "Пожалуйста, введите корректное место.")  
bot.polling(none_stop=True)
