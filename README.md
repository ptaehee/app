# app


from re import S                            #back to home
from kivy.app import App                    #make app
from kivy.base import runTouchApp           #for application (button, text, ect)
from kivy.lang import Builder               #버튼 위치
from kivy.properties import ListProperty    #
from kivy.uix.boxlayout import BoxLayout    #
from PIL import Image                       #이미지 넣기
from kivy.uix.screenmanager import ScreenManager, Screen, FadeTransition
                        # 스크린 모듈
import qrcode           #qr 모듈
from PIL import Image
import random 
import string
import time
import random 
import cv2
from queue import Queue
from threading import Thread
 #아르키 코드 카메라 추가

#리더기


class ChoiceScreen(Screen):
    pass
class SecondScreen(Screen):
    pass
class SellScreen(Screen):
    string_pool=string.digits
    result=""
    for i in range(8):
        result += random.choice(string_pool)
    img=qrcode.make(result)
    qr=qrcode.QRCode(version=1,error_correction=qrcode.constants.ERROR_CORRECT_H,box_size=10, border=4,)
    img.save("sample.png")
    cv2.imread('sample.png')
    
    
    
class MyScreenManager(ScreenManager):
    def new_qr_screen(self):
        name=id
        s =SellScreen(name=name,colour=[random.choice(string) for _ in range(8)] + [1])
        self.add_widget(s)
        self.current = name
    


class FirstScreen(Screen):
    def capture(self):
        camera = self.ids['camera']
        timestr = time.strftime("%Y%m%d_%H%M%S")
        camera.export_to_png("C:\\Users\\LeeJinHyeok\\Desktop\\PythonWorkspace\\imgtest\\IMG_{}.png".format(timestr))
    
    

        
root_widget = Builder.load_string('''
#:import FadeTransition kivy.uix.screenmanager.FadeTransition
MyScreenManager:
    transition: FadeTransition()
    ChoiceScreen:
    FirstScreen:
    SecondScreen:
        name: "second"
        id: second
    SellScreen: 
        name: 'third'

<ChoiceScreen>:
    name:'choice'
    BoxLayout:
        orientataion:'vertical'
        Image:
            source:'profile.png'
            size_hint:(0.5,0.3)
            pos_hint:{'center_x':0.25,'center_y':0.8}
    BoxLayout:
        orientation:'horizontal'
        Button: 
            text: 'consumer'
            font_size: 10
            size_hint:(0.5,0.3)
            pos_hint:{'center_x':0.25,'center_y':0.5}
            on_release: app.root.current = 'fish photo'
            background_color: 1,2.5,0.2,1
            Image:
                source:'consumer.png'
                center_x: self.parent.center_x
                center_y: self.parent.center_y  
        Button:
            text: 'seller'
            font_size: 10
            size_hint:(0.5,0.3)
            pos_hint:{'center_x':0.75,'center_y':0.5}
            on_release: app.root.current = 'third'
            background_color: 1,2.5,0.2,1
            Image:
                source:'seller.png'
                size_hint:(1,1) 
                center_x: self.parent.center_x
                center_y: self.parent.center_y  
        

<FirstScreen>:
    name: 'fish photo'
    BoxLayout:
        orientation: 'vertical'
        Label:
            text: 'fish photo'   #제목 이름
            font_size:  10       #글자 크기
            size_hint: .20,.20
            pos_hint: {'center_x': 0.5}
            Image:
                source: 'tittle.png'
                center_x:self.parent.center_x
                center_y:self.parent.center_y

        Camera:
            id: camera
            size: 300,200
            size_hint: None,None #
            pos_hint: {'center_x': 0.5}
            resolution: (1024,1024)
            camera1: False


        BoxLayout:
            orientation: 'horizontal'
            Image:
                source: 'fishname.png'
                size_hint: .4,.4
            #GridLayout:
                #size_hint_x: 1.5, 1.5
            Label:
                text:'Mackerel test'
                font_size: 18
        BoxLayout:
            orientation: 'horizontal'
            Image:
                source: 'length.png'
                size_hint: .4,.4
             #   GridLayout:
              #      size_hint_x: 1.5, 1.5
               #     size_hint_y: -5, -5
            Label:
                text: '20cm test'
                font_size: 18
        BoxLayout:
            orientation: 'horizontal'
            Image:
                source: 'width.png'
                size_hint: .4,.4
            Label:
                text: '8cm test'
                font_size: 18
        BoxLayout:
            orientation: 'horizontal'
            Image:
                source: 'kg.png'
                size_hint: .4,.4
            Label:
                text: '6,000won test'
                font_size: 18
        BoxLayout:
            orientation: 'horizontal'
            Image:
                source: 'registration.png'
                size_hint: .4,.4
            Label:
                text: '2021 July 23 test'
                font_size: 18

        BoxLayout:
            orientaiton: 'horizontal'
            Button:
                text: 'camera'
                font_size: 30
                size_hint: .30, .30
                background_color: 0,0,0,0
                on_press: camera.play=not camera.play, root.capture()
                Image:
                    source:'camera.png'
                    center_x: self.parent.center_x
                    center_y: self.parent.center_y
            Button:
                text: 'back'
                font_size:30
                size_hint:.30,.30
                on_release: app.root.current = 'choice'
                background_color: 0,0,0,0
                Image:
                    source:'back.png'
                    center_x: self.parent.center_x
                    center_y: self.parent.center_y
            Button:
                text: 'next'
                font_size: 30
                size_hint:.30, .30
                on_release: app.root.current = 'second'
                background_color: 0,0,0,0
                Image:
                    source:'right.png'
                    center_x: self.parent.center_x
                    center_y: self.parent.center_y  
            TextInput:
                size_hint: (.30, None)
                height: 50
                multiline: False
                text: ' '
            Button:
                text: 'search'
                font_size: 30
                size_hint:.30,.30
                background_color:0,0,0,0
                Image:
                    source:'lens.png'
                    center_x: self.parent.center_x
                    center_y: self.parent.center_y
            
                
<SecondScreen>:
    name: 'second'
    BoxLayout:
        orientation: 'vertical'
        Label:
            text: 'result of inquiry'
            font_size: 30
        BoxLayout:
            Button:
                text: 'default screen'
                font_size: 30
                size_hint:(0.4,0.3)
                 text: 'get qrcode'
                font_size: 30
                size_hint:(0.4,0.3)
                on_release: app.root.new_qr_screen()
<SellScreen>:
    name:'third'
    BoxLayout: 
        orientation: 'vertical'
        Label:
            text: 'qrcode'
            font_size: 30
        Image:
            source:'sample.png'
            size:500,500
        BoxLayout:
            BoxLayout:
                BoxLayout:                  #밑에 첫번째 왼쪽 공백
                    size_hint: (0.1,0.4)
                Button:
                    text: 'default'
                    font_size: 30
                    size_hint: (0.3,0.4)
                    background_color:0,0,0,0
                    on_release: app.root.current = 'choice'  # 돌아가는 스크린 이름
                    Image:
                        source:'back.png'
                        center_x: self.parent.center_x
                        center_y: self.parent.center_y
                BoxLayout:                  
                    size_hint: (0.3,0.4)
                
                Button:
                    text: 'get qr'
                    font_size: 30
                    on_release: app.root.new_qr_screen()
                    size_hint: (0.3,0.4)
                BoxLayout:                  #밑에 첫번째 왼쪽 공백
                    size_hint: (0.1,0.4)
    
''')
 
class ScreenManagerApp(App):
    def build(self):
        return root_widget
 
ScreenManagerApp().run()
