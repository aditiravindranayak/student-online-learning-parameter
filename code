import cv2
import cvzone
from cvzone.FaceMeshModule import FaceMeshDetector
from cvzone.PlotModule import LivePlot
from tkinter import *
from tkinter import messagebox
from tkinter import ttk
import sys
import os
import mysql.connector

#  python -m PyInstaller test.py --onefile --windowed --hiddenimport cvzone

myconn = mysql.connector.connect(host='localhost', database='sit', user='root', password='12345')
mycursor = myconn.cursor()


def NameCalibrate(v_name2):
    v_name3 = ""
    v_name4 = ""

    l1 = len(v_name2)
    for i in range(l1):

        if v_name2[i] == chr(92):
            v_name3 = v_name3 + chr(92) + chr(92)
        else:
            v_name3 = v_name3 + v_name2[i]

    lst2 = ['\x07', '\x08', '\x0c', '\n', ' \r', '\t', '\x0b']

    l2 = len(v_name3)
    l3 = len(lst2)
    i = 0
    while (i < l2):
        for j in range(l3):
            if v_name3[i] == lst2[j]:
                if j == 0:
                    v_name4 = v_name4 + chr(92) + chr(92) + 'a'
                    i = i + 1
                elif j == 1:
                    v_name4 = v_name4 + chr(92) + chr(92) + 'b'
                    i = i + 1
                elif j == 2:
                    v_name4 = v_name4 + chr(92) + chr(92) + 'f'
                    i = i + 1
                elif j == 3:
                    v_name4 = v_name4 + chr(92) + chr(92) + 'n'
                    i = i + 1
                elif j == 4:
                    v_name4 = v_name4 + chr(92) + chr(92) + 'r'
                    i = i + 1
                elif j == 5:
                    v_name4 = v_name4 + chr(92) + chr(92) + 't'
                    i = i + 1
                elif j == 6:
                    v_name4 = v_name4 + chr(92) + chr(92) + 'v'
                    i = i + 1

        else:
            v_name4 = v_name4 + v_name3[i]
            i = i + 1
    return v_name4

def resource_path(relative_path):
    try:
        base_path = sys._MEIPASS
    except Exception:
        base_path = os.path.abspath(".")

    return os.path.join(base_path, relative_path)

def FrameDecider(blinks, yawns, muscles, tframes ):

    v_name = e_vid.get()
    fps = 30
    tduration = tframes/fps
    blinkAvgMin = 13

    blinkAvg = blinkAvgMin * (tduration/60)

    if blinks > blinkAvg:
        flag = 0
        # INSERT INTO DATABASE AND PRINT LOWER THAN AVERAGE CONCENTRATION
    else:
        flag = 1
        # INSERT INTO DATABASE AND PRINT HIGHER THAN AVERAGE CONCENTRATION

    for i in yawns:
        t1 = round(i/60,2)
        if (t1-2) < 0:
            tr = f"INSERT INTO PBL(V_NAME, DF_TIME, DT_TIME) VALUES ('{v_name}','{t1}','{t1+2}') "
            mycursor.execute(tr)
            myconn.commit()
        else:
            tr = f"INSERT INTO PBL(V_NAME, DF_TIME, DT_TIME) VALUES ('{v_name}','{t1}','{t1 + 2}') "
            mycursor.execute(tr)
            myconn.commit()


    for j in muscles:
        t2 = round(j/60,2)

        if (t2-1 ) < 0:
            tr2 = f"INSERT INTO PBL(V_NAME, CF_TIME, CT_TIME) VALUES ('{v_name}','{t2}','{t2 + 1}') "
            mycursor.execute(tr2)
            myconn.commit()
        else:

            tr2 = f"INSERT INTO PBL(V_NAME, CF_TIME, CT_TIME) VALUES ('{v_name}','{t2-1}','{t2 + 1}') "
            mycursor.execute(tr2)
            myconn.commit()


    return



def BlinkCounter(v_name):

    v_name2 = resource_path(v_name)
    v_name2 = NameCalibrate(v_name2)
    cap = cv2.VideoCapture(v_name2)
    detector = FaceMeshDetector(maxFaces=1)
    plotY = LivePlot(720, 480, [20, 50], invert=True)
    plotL = LivePlot(720,480, [0,25], invert=True)
    plotM = LivePlot(720, 480, [-2, 5], invert=True)

    idList = [22, 23, 24, 26, 110, 157, 158, 159, 160, 161, 130, 243]
    lpList = [12,14,76, 292]
    eyebrow = [8, 9, 107, 285, 336,]

    eyebrowMax = 0
    eyebrowAvg = {}
    ratioList = []

    blinkCounter = 0
    yawnList = []
    focusList = []

    color = (255, 0, 255)
    color2 = (0, 60, 0)
    color3 = (255, 255, 255)

    counter = 0
    flag = 0
    frame3 = 1

    while True:

        if cap.get(cv2.CAP_PROP_POS_FRAMES) == cap.get(cv2.CAP_PROP_FRAME_COUNT):

            tframe = int(cap.get(cv2.CAP_PROP_FRAME_COUNT))
            FrameDecider(blinkCounter, yawnList, focusList, tframe)

            return

        success, img = cap.read()
        img, faces = detector.findFaceMesh(img, draw=False)

        if faces:
            face = faces[0]
            for id in idList:
                cv2.circle(img, face[id], 5,color, cv2.FILLED)
            for id2 in lpList:
                cv2.circle(img, face[id2], 5, color2, cv2.FILLED)
            for id3 in eyebrow:
                cv2.circle(img, face[id3], 5, color3, cv2.FILLED)


        #yawn counter start ----------------------------------------------------

            lpUp = face[12]
            lpDown = face[14]
            lpLeft = face[76]
            lpRight = face[292]


           # lpWidth , _ = detector.findDistance(lpRight,lpLeft)
            cv2.line(img, lpRight, lpLeft, (0, 200, 0), 3)
            lpLength, _ = detector.findDistance(lpUp, lpDown)
            cv2.line(img, lpUp, lpDown, (0, 200, 0), 3)



            if lpLength > 25:
                frame1 = cap.get(cv2.CAP_PROP_POS_FRAMES)
                flag = 1
            if lpLength < 15 and flag == 1:
                frame2 = cap.get(cv2.CAP_PROP_POS_FRAMES)
                flag = 0
                yawnList.append(frame1)

            imgPlot2 = plotL.update(lpLength, color2)
            img = cv2.resize(img, (720, 480))



        # yawn counter end --------------------------------------------------------

            eyebrowUpLeft = face[107]
            eyebrowDnRight = face[285]
            eyebrowUpRight = face[336]

            eyebrowLength, _ = detector.findDistance(eyebrowUpRight,eyebrowUpLeft)
            cv2.line(img, eyebrowUpRight, eyebrowUpLeft, (255, 255, 255), 3)

            if (cap.get(cv2.CAP_PROP_POS_FRAMES) == frame3 + 1):
                eyebrowMax = eyebrowLength - eyebrowAvg[1]
                if eyebrowMax > 2:
                    focusList.append(frame3+1)

            frame3 = cap.get(cv2.CAP_PROP_POS_FRAMES)

            eyebrowAvg[0] = frame3
            eyebrowAvg[1] = eyebrowLength

            imgPlot3 = plotM.update(eyebrowMax, color3)
            img = cv2.resize(img, (720, 480))

        # eyebrow/ face muscle end --------------------------------------------------

            leftUp = face[159]
            leftDown = face[23]
            leftLeft = face[130]
            leftRight = face[243]
            lenghtVer, _ = detector.findDistance(leftUp, leftDown)
            lenghtHor, _ = detector.findDistance(leftLeft, leftRight)

            cv2.line(img, leftUp, leftDown, (0, 200, 0), 3)
            cv2.line(img, leftLeft, leftRight, (0, 200, 0), 3)

            ratio = int((lenghtVer / lenghtHor) * 100)
            ratioList.append(ratio)
            if len(ratioList) > 3:
                ratioList.pop(0)
            ratioAvg = sum(ratioList) / len(ratioList)


            if ratioAvg < 35 and counter == 0:
                blinkCounter += 1
                color = (0,200,0)
                counter = 1
            if counter != 0:
                counter += 1
                if counter > 10:
                    counter = 0
                    color = (255,0, 255)



            imgPlot = plotY.update(ratioAvg, color)
            # cvzone.putTextRect(img, f'Blink Count: {blinkCounter}', (150, 100), colorR=color)
            img = cv2.resize(img, (720, 480))
            imgStack = cvzone.stackImages([img, imgPlot, imgPlot2, imgPlot3], 2, 1)
        else:
            img = cv2.resize(img, (720, 480))
            imgStack = cvzone.stackImages([img, img], 2, 1)

        # blink counter end -------------------------------------------------

        # cv2.imshow("All Details", imgStack)
        cv2.imshow("Image", img)
        cv2.imshow("Blink Counter", imgPlot)
        cv2.imshow("Yawn ", imgPlot2)
        cv2.imshow("Face Muscles ", imgPlot3)
        cv2.waitKey(1)



def Records():
    v_name = e_vid.get()
    rec_win = Tk()
    rec_win.configure(bg='grey')
    rec_win.title('Recorded Data')
    rec_win.geometry("700x480")
    tv = ttk.Treeview(rec_win)

    tv['columns'] = ('Name', 'Yawned From', 'Yawned To', 'Concentrated From', "Concentrated To")


    tv.column("#0", width=0, stretch=0)
    tv.column("Name", anchor=CENTER, width=100)
    tv.column("Yawned From", anchor=CENTER, width=150)
    tv.column("Yawned To", anchor=CENTER, width=150)
    tv.column("Concentrated From", anchor=CENTER, width=150)
    tv.column("Concentrated To", anchor=CENTER, width=150)

    tv.heading("#0", text="", anchor=CENTER)
    tv.heading("Name", text="Name", anchor=CENTER)
    tv.heading("Yawned From", text="Yawned From", anchor=CENTER)
    tv.heading("Yawned To", text="Yawned To", anchor=CENTER)
    tv.heading("Concentrated From", text="Concentrated From", anchor=CENTER)
    tv.heading("Concentrated To", text="Concentrated To", anchor=CENTER)

    if v_name == "":
        sql = "Select * from PBL"
    else:
        sql = f"Select * from PBL where V_NAME = '{v_name}'"
    mycursor.execute(sql)
    rows = mycursor.fetchall()
    for i in rows:
        tv.insert('', 'end', values=i)

    tv.pack()


def exit():
    window.destroy()

def Process():

    v_name = e_vid.get()
    BlinkCounter(v_name + ".mp4")
    messagebox.showinfo("VIDEO STATUS", "VIDEO PROCESSED.")
    window.destroy()
    main()
def main():

    global window
    window = Tk()
    window.title('VIDEO ASSESSMENT')
    window.configure(bg='grey')
    window.geometry("400x470")
    window.resizable(width=False, height=False)
    Label(window, text="Video Details", width=15, font=('ariel', 19, 'bold'), relief="solid").place(x=85, y=25)
    lbl_vid = Label(window, width=22, font=("ariel", 10, "bold"), bg="grey", text="ENTER THE VIDEO NAME      :", )


    global e_vid


    e_vid = Entry(window)

    btn_signin = Button(window, text="Exit", width=12, bg="brown", fg="white", font=("bold", 10), command=exit)
    btn_back = Button(window, text="Process", width=12, bg="brown", fg="white", font=("bold", 10), command=Process)
    btn_data = Button(window, text="Records ", width=12, bg="brown", fg="white", font=("bold", 10), command=Records)
    lbl_vid.place(x=30, y=149)


    e_vid.place(x=220, y=150)


    btn_signin.place(x=220, y=250)
    btn_back.place(x=90, y=250)
    btn_data.place(x=150, y=300)



main()
window.mainloop()
