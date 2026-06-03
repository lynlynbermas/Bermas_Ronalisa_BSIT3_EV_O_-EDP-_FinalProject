Imports System.Drawing
Imports System.Windows.Forms

Public Class Form1

    Private WithEvents moveTimer As New Timer()

    Private carX As Integer = 20
    Private carSpeed As Integer = 5

    ' 0 = Green
    ' 1 = Yellow
    ' 2 = Red
    Private lightState As Integer = 0

    Private btnGreen As New Button()
    Private btnYellow As New Button()
    Private btnRed As New Button()

    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load

        Me.Text = "Traffic Light Simulator"
        Me.Width = 1000
        Me.Height = 700
        Me.DoubleBuffered = True

        ' GREEN BUTTON
        btnGreen.Text = "GREEN"
        btnGreen.Size = New Size(100, 40)
        btnGreen.Location = New Point(20, 20)
        AddHandler btnGreen.Click, AddressOf Green_Click
        Me.Controls.Add(btnGreen)

        ' YELLOW BUTTON
        btnYellow.Text = "YELLOW"
        btnYellow.Size = New Size(100, 40)
        btnYellow.Location = New Point(20, 70)
        AddHandler btnYellow.Click, AddressOf Yellow_Click
        Me.Controls.Add(btnYellow)

        ' RED BUTTON
        btnRed.Text = "RED"
        btnRed.Size = New Size(100, 40)
        btnRed.Location = New Point(20, 120)
        AddHandler btnRed.Click, AddressOf Red_Click
        Me.Controls.Add(btnRed)

        moveTimer.Interval = 30
        moveTimer.Start()

    End Sub

    Private Sub Green_Click(sender As Object, e As EventArgs)

        lightState = 0
        carSpeed = 5

        Me.Invalidate()

    End Sub

    Private Sub Yellow_Click(sender As Object, e As EventArgs)

        lightState = 1
        carSpeed = 2

        Me.Invalidate()

    End Sub

    Private Sub Red_Click(sender As Object, e As EventArgs)

        lightState = 2
        carSpeed = 0

        Me.Invalidate()

    End Sub

    Private Sub moveTimer_Tick(sender As Object, e As EventArgs) Handles moveTimer.Tick

        carX += carSpeed

        If carX > Me.ClientSize.Width Then
            carX = -100
        End If

        Me.Invalidate()

    End Sub

    Protected Overrides Sub OnPaint(e As PaintEventArgs)

        MyBase.OnPaint(e)

        Dim g As Graphics = e.Graphics

        ' GRASS
        g.Clear(Color.ForestGreen)

        ' HORIZONTAL ROAD
        g.FillRectangle(Brushes.DimGray, 0, 300, Me.Width, 120)

        ' VERTICAL ROAD
        g.FillRectangle(Brushes.DimGray, 450, 0, 120, Me.Height)

        ' ROAD MARKINGS
        Dim p As New Pen(Color.White, 3)
        p.DashPattern = New Single() {10, 10}

        g.DrawLine(p, 0, 360, Me.Width, 360)
        g.DrawLine(p, 510, 0, 510, Me.Height)

        ' STOP LINE
        g.DrawLine(New Pen(Color.White, 5), 430, 300, 430, 420)

        ' ==================
        ' TRAFFIC LIGHT
        ' ==================

        g.FillRectangle(Brushes.Black, 650, 150, 70, 190)

        Dim redBrush As Brush = Brushes.DarkRed
        Dim yellowBrush As Brush = Brushes.Olive
        Dim greenBrush As Brush = Brushes.DarkGreen

        If lightState = 2 Then redBrush = Brushes.Red
        If lightState = 1 Then yellowBrush = Brushes.Yellow
        If lightState = 0 Then greenBrush = Brushes.Lime

        g.FillEllipse(redBrush, 670, 165, 30, 30)
        g.FillEllipse(yellowBrush, 670, 220, 30, 30)
        g.FillEllipse(greenBrush, 670, 275, 30, 30)

        ' ==================
        ' CAR
        ' ==================

        DrawCar(g, carX, 330)

    End Sub

    Private Sub DrawCar(g As Graphics, x As Integer, y As Integer)

        ' BODY
        g.FillRectangle(Brushes.RoyalBlue, x, y, 80, 30)

        ' ROOF
        g.FillRectangle(Brushes.LightBlue, x + 15, y + 5, 40, 20)

        ' WHEELS
        g.FillRectangle(Brushes.Black, x + 10, y - 3, 15, 5)
        g.FillRectangle(Brushes.Black, x + 55, y - 3, 15, 5)

        g.FillRectangle(Brushes.Black, x + 10, y + 28, 15, 5)
        g.FillRectangle(Brushes.Black, x + 55, y + 28, 15, 5)

        ' HEADLIGHTS
        g.FillEllipse(Brushes.Yellow, x + 72, y + 5, 5, 5)
        g.FillEllipse(Brushes.Yellow, x + 72, y + 20, 5, 5)

    End Sub

End Class
