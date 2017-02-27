unit Unit2;

{$mode objfpc}{$H+}

interface

uses
  Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, ExtCtrls,
  StdCtrls, Math;//Math добавлят Ceil и Floor

type

  { TForm2 }

  TForm2 = class(TForm)
    Button1: TButton;
    Button2: TButton;
    Edit1: TEdit;
    Image1: TImage;
    Image2: TImage;
    Image3: TImage;
    Image4: TImage;
    Label1: TLabel;
    Label2: TLabel;
    Label3: TLabel;
    Timer1: TTimer;
    procedure Button1Click(Sender: TObject);
    procedure Button2Click(Sender: TObject);
    procedure FormCreate(Sender: TObject);
    procedure FormKeyDown(Sender: TObject; var Key: Word);
    procedure Image2Click(Sender: TObject);
    procedure Timer1Timer(Sender: TObject);
  private
    { private declarations }
  public
    { public declarations }
  end;

var
  Form2: TForm2;
  victory:boolean;
implementation

{$R *.lfm}

{ TForm2 }
uses unit1;
procedure TForm2.FormCreate(Sender: TObject);
var i,j,k,err,rec:integer;
begin
  //timer1.Enabled:=true;
  randomize;
  k:=0;
  for i:=1 to unit1.mas do
    for j:=1 to unit1.mas do
      begin
        if unit1.Map[i,j].Spawn = true then
          begin
            if unit1.nummap = 1 then
             unit1.PacMan:=TPacMan.Create(i*unit1.lol,j*unit1.lol)
            else
             begin
             Pacman.x:=i*unit1.lol;
             Pacman.y:=j*unit1.lol;
             Pacman.direction2:='Stay';
             Pacman.direction1:='Stay';
             pacman.speed:=unit1.pacsp;
             end;
          end;
        if unit1.Map[i,j].GSpawn = true then
          begin
            k:=k+1;
            unit1.Ghosts[k]:=TGhost.Create(i*unit1.lol,j*unit1.lol);
            unit1.ghosts[k].speed:=unit1.ghosp;
          end;
      end;
  form2.Timer1.Interval:=unit1.gamesp;
  form2.Width:=unit1.mas*unit1.lol;
  form2.Height:=unit1.mas*unit1.lol+50;
  form2.Image1.height:=unit1.mas*unit1.lol;
  form2.image1.width:=unit1.mas*unit1.lol;  //не отображается
end;

procedure TForm2.FormKeyDown(Sender: TObject; var Key: Word);
begin
  case key of
    40:pacman.direction2:='up';
    39:pacman.direction2:='right';
    38:pacman.direction2:='down';
    37:pacman.direction2:='left';
  end;
end;

procedure TForm2.Image2Click(Sender: TObject);
begin

end;

procedure TForm2.Timer1Timer(Sender: TObject);
var s,sf:string;
    k,i,j,rand,n:integer;
    choose,see:boolean;
    f:textfile;
begin
  if (pacman.life <> 0) and (unit2.victory = false) then
 begin
  for i:=1 to unit1.mas do
    for j:=1 to unit1.mas do
        if unit1.Map[i,j].point.eat = false then
            case unit1.Map[i,j].point.tip of
              'normal':begin
                         image1.canvas.brush.Color:=clwhite;
                         image1.Canvas.Ellipse((i-1)*unit1.lol+7,(j-1)*unit1.lol+7,i*unit1.lol-7,j*unit1.lol-7);
                       end;
              'super':begin
                        image1.canvas.brush.Color:=clwhite;
                        image1.Canvas.Ellipse((i-1)*unit1.lol+4,(j-1)*unit1.lol+4,i*unit1.lol-4,j*unit1.lol-4);
                      end;
              '+life':begin
                        image1.canvas.brush.Color:=clFuchsia;
                        image1.Canvas.Ellipse((i-1)*unit1.lol+4,(j-1)*unit1.lol+4,i*unit1.lol-4,j*unit1.lol-4);
                      end;
              '+immune':begin
                          image1.canvas.brush.Color:=clLime;
                          image1.Canvas.Ellipse((i-1)*unit1.lol+4,(j-1)*unit1.lol+4,i*unit1.lol-4,j*unit1.lol-4);
                        end;
            end;
  Pacman.UnShow(Image1);

  //----------------------------------------------------------------------------

  for k:=1 to kolvo do
    begin
      Ghosts[k].UnShow(Image1);

      //ИИ
      if (ghosts[k].x mod unit1.lol <> 0)or(ghosts[k].y mod unit1.lol <> 0) then
      begin
        case ghosts[k].direction of
          'up':ghosts[k].y:=ghosts[k].y+ghosts[k].speed;
          'right':ghosts[k].x:=ghosts[k].x+ghosts[k].speed;
          'down':ghosts[k].y:=ghosts[k].y-ghosts[k].speed;
          'left':ghosts[k].x:=ghosts[k].x-ghosts[k].speed;
    end;
      end  //для всех условие одинаково
      else
      begin
        {if Map[round(ghosts[k].x/unit1.lol),round(ghosts[k].y/unit1.lol)].point.eat = false then
      begin
        Map[round(ghosts[k].x/unit1.lol),round(ghosts[k].y/unit1.lol)].point.eat:= true;
      end;}
        choose:=false;
        n:=0;
       repeat         //&
        rand:=1+random(4);
        case rand of
          1:ghosts[k].direction:='up';
          2:ghosts[k].direction:='down';
          3:ghosts[k].direction:='left';
          4:ghosts[k].direction:='right';
        end;
        //следование за игроком
        if ghosts[k].x=pacman.x then
          if ghosts[k].y<pacman.y then
          begin
            see:=true;
            for i:=round(ghosts[k].y/unit1.lol) to round(pacman.y/unit1.lol) do
              if Map[Ceil(pacman.x/unit1.lol),i].wall = true then
                see:=false;
              if see = true then
              begin
                ghosts[k].direction:='up';
                ghosts[k].xb:=ghosts[k].x;
                ghosts[k].yb:=ghosts[k].y;
                ghosts[k].y:=ghosts[k].y+ghosts[k].speed;
                choose:=true;
              end;
          end
          else
          begin
            see:=true;
            for i:=round(pacman.y/unit1.lol) to round(ghosts[k].y/unit1.lol) do
              if Map[Floor(pacman.x/unit1.lol),i].wall = true then
                see:=false;
              if see = true then
              begin
                ghosts[k].direction:='down';
                ghosts[k].xb:=ghosts[k].x;
                ghosts[k].yb:=ghosts[k].y;
                ghosts[k].y:=ghosts[k].y-ghosts[k].speed;
                choose:=true;
              end;
          end;
        if ghosts[k].y=pacman.y then
          if ghosts[k].x<pacman.x then
          begin
            see:=true;
            for i:=round(ghosts[k].x/unit1.lol) to round(pacman.x/unit1.lol) do
              if Map[i,Ceil(pacman.y/unit1.lol)].wall = true then
                see:=false;
              if see = true then
              begin
                ghosts[k].direction:='right';
                ghosts[k].xb:=ghosts[k].x;
                ghosts[k].yb:=ghosts[k].y;
                ghosts[k].x:=ghosts[k].x+ghosts[k].speed;
                choose:=true;
              end;
          end
          else
          begin
            see:=true;
            for i:=round(pacman.x/unit1.lol) to round(ghosts[k].x/unit1.lol) do
              if Map[i,Floor(pacman.y/unit1.lol)].wall = true then
                see:=false;
              if see = true then
              begin
                ghosts[k].direction:='left';
                ghosts[k].xb:=ghosts[k].x;
                ghosts[k].yb:=ghosts[k].y;
                ghosts[k].x:=ghosts[k].x-ghosts[k].speed;
                choose:=true;
              end;
          end;
          //конец следованния
        if choose = false then
        case ghosts[k].direction of
          'up':if (Map[round(ghosts[k].x/unit1.lol),round(ghosts[k].y/unit1.lol)+1].wall = false) and (((ghosts[k].x=ghosts[k].xb) and (ghosts[k].y+unit1.lol=ghosts[k].yb)) = false) then
          begin
            ghosts[k].xb:=ghosts[k].x;
            ghosts[k].yb:=ghosts[k].y;
            ghosts[k].y:=ghosts[k].y+ghosts[k].speed;
            choose:=true;
          end;
          'down':if (Map[round(ghosts[k].x/unit1.lol),round(ghosts[k].y/unit1.lol)-1].wall = false) and (((ghosts[k].x=ghosts[k].xb) and (ghosts[k].y-unit1.lol=ghosts[k].yb)) = false) then
          begin
            ghosts[k].xb:=ghosts[k].x;
            ghosts[k].yb:=ghosts[k].y;
            ghosts[k].y:=ghosts[k].y-ghosts[k].speed;
            choose:=true;
          end;
          'left':if (Map[round(ghosts[k].x/unit1.lol)-1,round(ghosts[k].y/unit1.lol)].wall = false) and (((ghosts[k].x-unit1.lol=ghosts[k].xb) and (ghosts[k].y=ghosts[k].yb)) = false) then
          begin
            ghosts[k].xb:=ghosts[k].x;
            ghosts[k].yb:=ghosts[k].y;
            ghosts[k].x:=ghosts[k].x-ghosts[k].speed;
            choose:=true;
          end;
          'right':if (Map[round(ghosts[k].x/unit1.lol)+1,round(ghosts[k].y/unit1.lol)].wall = false) and (((ghosts[k].x+unit1.lol=ghosts[k].xb) and (ghosts[k].y=ghosts[k].yb)) = false) then
          begin
            ghosts[k].xb:=ghosts[k].x;
            ghosts[k].yb:=ghosts[k].y;
            ghosts[k].x:=ghosts[k].x+ghosts[k].speed;
            choose:=true;
          end;
        end;
        //-----------------------------------
        n:=n+1;
        if n = 10 then
          begin
            ghosts[k].xb:=-1;
            ghosts[k].yb:=-1;
          end;
        //-----------------------------------
        until choose = true;
      end;

      Ghosts[k].Show(Image1);
    end;

  //----------------------------------------------------------------------------

  if (pacman.x mod unit1.lol <> 0)or(pacman.y mod unit1.lol <> 0) then
  begin
    case pacman.direction1 of
      'up':pacman.y:=pacman.y+pacman.speed ;
      'right':pacman.x:=pacman.x+pacman.speed ;
      'down':pacman.y:=pacman.y-pacman.speed ;
      'left':pacman.x:=pacman.x-pacman.speed ;
    end;
  end
  else
  begin
    if Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.eat = false then
        case Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.tip of
        'super':begin
                   pacman.score:=pacman.score+Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.number;
                   Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.eat:= true;
                end;
        'normal':begin
                   pacman.score:=pacman.score+Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.number;
                   Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.eat:= true;
                 end;
        '+life':if pacman.life < 3 then
                begin
                  pacman.score:=pacman.score+Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.number;
                  Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.eat:= true;
                  pacman.life:=pacman.life+1;
                end
                else
                begin
                  pacman.score:=pacman.score+Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.number*5;
                  Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.eat:= true;
                end;
        '+immune':begin
                   pacman.score:=pacman.score+Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.number*5;
                   Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)].point.eat:= true;
                   pacman.immune:=pacman.immune+100;
                 end;
        end;
   { case pacman.direction1 of
      'up':if Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)+1].wall = false then pacman.y:=pacman.y+pacman.speed else pacman.direction1:='stay';
      'right':if Map[round(pacman.x/unit1.lol)+1,round(pacman.y/unit1.lol)].wall = false then pacman.x:=pacman.x+pacman.speed else pacman.direction1:='stay';
      'down':if Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)-1].wall = false then pacman.y:=pacman.y-pacman.speed else pacman.direction1:='stay';
      'left':if Map[round(pacman.x/unit1.lol)-1,round(pacman.y/unit1.lol)].wall = false then pacman.x:=pacman.x-pacman.speed else pacman.direction1:='stay';
    end;}//после

    pacman.direction1:=pacman.direction2;
    str(pacman.score,s);
    Edit1.Caption:=s;   // -----


    case pacman.direction1 of
      'up':if Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)+1].wall = false then pacman.y:=pacman.y+pacman.speed else pacman.direction1:='stay';
      'right':if Map[round(pacman.x/unit1.lol)+1,round(pacman.y/unit1.lol)].wall = false then pacman.x:=pacman.x+pacman.speed else pacman.direction1:='stay';
      'down':if Map[round(pacman.x/unit1.lol),round(pacman.y/unit1.lol)-1].wall = false then pacman.y:=pacman.y-pacman.speed else pacman.direction1:='stay';
      'left':if Map[round(pacman.x/unit1.lol)-1,round(pacman.y/unit1.lol)].wall = false then pacman.x:=pacman.x-pacman.speed else pacman.direction1:='stay';
    end;//до
  end;
  Pacman.Show(Image1);
  for k:=1 to unit1.kolvo do
    if (sqr(pacman.x-ghosts[k].x)+sqr(pacman.y-ghosts[k].y)<=sqr(lol-5)) and (pacman.immune = 0) then
    begin
      Pacman.unshow(Image1);
      pacman.life:=pacman.life-1;
      //if pacman.score>0 then
      //  pacman.score:=round(pacman.score-0.25*pacman.score);
      pacman.direction2:='stay';
      pacman.direction1:='stay';
      pacman.immune:=50;
      for i:=1 to unit1.mas do
        for j:=1 to unit1.mas do
          begin
            if Map[i,j].Spawn = true then
              begin
                PacMan.x:=i*unit1.lol;
                PacMan.y:=j*unit1.lol;
              end;
          end;
    end;
  //переход в противоположную часть карты
  //pacman
  if pacman.x = unit1.mas*unit1.lol-pacman.speed+unit1.lol then
    begin
    pacman.UnShow(Image1);
    pacman.x:=pacman.speed;
    end
  else
  if pacman.x = pacman.speed then
    begin
    pacman.UnShow(Image1);
    pacman.x:= unit1.mas*unit1.lol-pacman.speed+unit1.lol;
    end;
   if pacman.y = unit1.mas*unit1.lol-pacman.speed+unit1.lol then
    begin
    pacman.UnShow(Image1);
    pacman.y:=pacman.speed;
    end
  else
  if pacman.y = pacman.speed then
    begin
    pacman.UnShow(Image1);
    pacman.y:= unit1.mas*unit1.lol-pacman.speed+unit1.lol;
    end;
  //ghosts
  for k:=1 to unit1.kolvo do
    begin
      if ghosts[k].x = unit1.mas*unit1.lol-ghosts[k].speed+unit1.lol then
    begin
    ghosts[k].UnShow(Image1);
    ghosts[k].x:=ghosts[k].speed;
    end
  else
  if ghosts[k].x = ghosts[k].speed then
    begin
    ghosts[k].UnShow(Image1);
    ghosts[k].x:= unit1.mas*unit1.lol-ghosts[k].speed+unit1.lol;
    end;
   if ghosts[k].y = unit1.mas*unit1.lol-ghosts[k].speed+unit1.lol then
    begin
    ghosts[k].UnShow(Image1);
    ghosts[k].y:=ghosts[k].speed;
    end
  else
  if ghosts[k].y = ghosts[k].speed then
    begin
    ghosts[k].UnShow(Image1);
    ghosts[k].y:= unit1.mas*unit1.lol-ghosts[k].speed+unit1.lol;
    end;
    end;
  //аналогично для остальных приведений
  //
  if pacman.life = 3 then
  begin
    image2.Visible:=true;
    image3.Visible:=true;
    image4.Visible:=true;
  end;
  if pacman.life = 2 then
  begin
    image2.Visible:=true;
    image3.Visible:=true;
    image4.Visible:=false;
  end;
  if pacman.life = 1 then
  begin
    image2.Visible:=true;
    image3.Visible:=false;
    image4.Visible:=false;
  end;
  if pacman.life = 0 then
  begin
    image2.Visible:=false;
    image3.Visible:=false;
    image4.Visible:=false;
    form2.Edit1.caption:='You are dead!';
    pacman.free;
    for k:=1 to kolvo do
      ghosts[k].free;
  end;
  if (pacman.immune<>0) and (pacman.life<>0) then
  begin
    pacman.immune:=pacman.immune-1;
    str(pacman.immune,s);
    form2.Edit1.Caption:='immune '+s;
  end;
  unit2.victory:= true;   //вернуть
    for i:=1 to unit1.mas do
      for j:=1 to unit1.mas do
        begin
        if map[i,j].wall = true then
          begin
            image1.canvas.brush.Color:=clRed;
            image1.Canvas.Rectangle((i-1)*unit1.lol,(j-1)*unit1.lol,i*unit1.lol,j*unit1.lol);;
          end;
          //условие победы
          if map[i,j].point.eat = false then
            unit2.victory:= false
        end;
    if victory = true then
      if nummap <5 then
      begin
        image1.canvas.Brush.Color:=clblack;
        image1.Canvas.Clear;
        for i:=1 to unit1.mas do
          for j:=1 to unit1.mas do
            begin
              map[j,i].wall:=false;
              map[j,i].point.eat:=true;
              map[j,i].spawn:=false;
              map[j,i].gspawn:=false;
            end;
        pacman.immune:=0;
        for k:=1 to unit1.kolvo do
          ghosts[k].Free();
        unit1.readi(unit1.Map);
        form2.FormCreate(Sender);
        victory:=false;
      end
    else
    begin
      str(pacman.score,s);
      form2.Edit1.Caption:='Score = '+s;
    end;
 end;
end;


procedure TForm2.Button1Click(Sender: TObject);
begin
  form1.Close;
end;

procedure TForm2.Button2Click(Sender: TObject);
begin
  if form2.Button2.Caption = 'Pause' then
  begin
    form2.button2.caption:='Reset';
    form2.Timer1.Enabled:=false;
  end
  else
  begin
    form2.button2.caption:='Pause';
    form2.Timer1.Enabled:=true;
  end;
end;

end.

