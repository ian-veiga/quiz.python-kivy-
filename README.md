from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.button import Button
from kivy.uix.label import Label
from kivy.uix.popup import Popup
from kivy.clock import  Clock

                                                          

class AnimeQuiz(App):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.time_left = 30
        self.clock_id = Clock.schedule_interval(self.update_timer, 1)

    def update_timer(self, df):
        self.time_left -= 1
        self.timer_label.text = str(self.time_left)
        if self.time_left <= 0:
           Clock.unschedule(self.clock_id)
           self.encerrar()
            
       
        
    
    

    def build(self):
        self.index = 0
        self.score = 0

        self.questions = [{"question": "seja bem vindo ao quiz de animes :)", "options": [ "START" ], "correct": "START"},
            {"question": "Qual é o protagonista de Naruto?", "options": ["Naruto Uzumaki", "Sasuke Uchiha", "Kakashi Hatake"], "correct": "Naruto Uzumaki"},
            {"question": "Em qual cidade Death Note se passa?", "options": ["Tóquio", "Nova York", "Londres"], "correct": "Tóquio"},
            {"question": "Qual o nome do renomado rei dos piratas em one piece?", "options": ["Luffy", "Shanks", "Roger"], "correct": "Roger"},
            {"question": "Em hunter x hunter qual o sobrenome da familia do persogem killua?", "options": ["Zoldick", "Silva", "D."], "correct": "Zoldick"}, 
            {"question": "Qual é o título em inglês do anime japonês conhecido como Shingeki no Kyojin?", "options": ["Fullmetal Alchemist", "Attack on Titan", "My Hero Academia"], "correct": "Attack on Titan"},
            {"question": "Quais o número escritos nos bottons do gon e killua (respectivamente) no exame hunter?  ", "options": ["(105/87)", "(905/49)", "(405/99)"], "correct": "(405/99)"},
            {"question": "Qual a maior medo do GOKU?", "options": ["Perder a chichi(esposa)", "Agulha", "Bills(deus da destruição)"], "correct": "Agulha"}
            ] 
        self.listabotoes = []
        self.layout = BoxLayout(orientation= 'vertical')
        self.question_label = Label(text=self.questions[self.index]["question"])
        self.layout.add_widget(self.question_label)
        self.timer_label = Label(text=str(self.time_left))
        self.layout.add_widget(self.timer_label)

        for option in self.questions[self.index]["options"]:
            btn = Button(text=option, on_press=self.check_answer)
            self.listabotoes.append(btn)
            self.layout.add_widget(btn)

        return self.layout

    def check_answer(self, instance):
        if instance.text == self.questions[self.index]["correct"]:
            self.score += 1

        self.index += 1

        if self.index < len(self.questions):
            self.update_question()
        
        else:
            self.show_score()

    def update_question(self):
        self.question_label.text = self.questions[self.index]["question"]

        for child in self.layout.children[0:]:
            if type(child) != Label:
              self.layout.remove_widget(child)
        self.listabotoes.clear()

        for option in self.questions[self.index]["options"]:
            btn = Button(text=option, on_press=self.check_answer)
            self.layout.add_widget(btn)
        
    def encerrar(self):
        score_popup = Popup(title='tempo esgotado :(',
                            content=Label(text=f'infelizmente seu tempo acabou,tente novamente :)'),
                            size_hint=(None, None), size=(500, 200))
                            

        score_popup.open()

    def show_score(self):
        score_popup = Popup(title='Quiz Concluído',
                            content=Label(text=f'Sua pontuação: {self.score -1}/{len(self.questions)-1}'),
                            size_hint=(None, None), size=(500, 200))
        

        score_popup.open()

if __name__ == '__main__':
        
        AnimeQuiz().run()
