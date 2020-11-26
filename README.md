# zigzag_pathplanning_ABB_robotstudio
MODULE Module1
    VAR num Path_Array{100,3};
    VAR num countit:=1;
    VAR num i;
    VAR num j;
    VAR num k;
    VAR num N;
    VAR num ivalue;
    VAR num Array_Pattern{5,5}:=[[0,0,0,0,0],[0,1,0,0,0],[0,0,0,0,0],[0,0,0,1,0],[0,1,0,0,0]];
    CONST robtarget Home_pos:=[[462.6545367,0,372.731777279],[0.000000001,0,1,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Start_pos:=[[462.654539737,-34.105263101,316.893339007],[-0.000000174,0,1,-0.000000003],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];

    PROC main()
        Array_Pattern{1,1}:=DINPUT(DiSensor11);
        Array_Pattern{1,2}:=DINPUT(DiSensor12);
        Array_Pattern{1,3}:=DINPUT(DiSensor13);
        Array_Pattern{1,4}:=DINPUT(DiSensor14);
        Array_Pattern{1,5}:=DINPUT(DiSensor15);
        Array_Pattern{2,1}:=DINPUT(DiSensor21);
        Array_Pattern{2,2}:=DINPUT(DiSensor22);
        Array_Pattern{2,3}:=DINPUT(DiSensor23);
        Array_Pattern{2,4}:=DINPUT(DiSensor24);
        Array_Pattern{2,5}:=DINPUT(DiSensor25);
        Array_Pattern{3,1}:=DINPUT(DiSensor31);
        Array_Pattern{3,2}:=DINPUT(DiSensor32);
        Array_Pattern{3,3}:=DINPUT(DiSensor33);
        Array_Pattern{3,4}:=DINPUT(DiSensor34);
        Array_Pattern{3,5}:=DINPUT(DiSensor35);
        Array_Pattern{4,1}:=DINPUT(DiSensor41);
        Array_Pattern{4,2}:=DINPUT(DiSensor42);
        Array_Pattern{4,3}:=DINPUT(DiSensor43);
        Array_Pattern{4,4}:=DINPUT(DiSensor44);
        Array_Pattern{4,5}:=DINPUT(DiSensor45);
        Array_Pattern{5,1}:=DINPUT(DiSensor51);
        Array_Pattern{5,2}:=DINPUT(DiSensor52);
        Array_Pattern{5,3}:=DINPUT(DiSensor53);
        Array_Pattern{5,4}:=DINPUT(DiSensor54);
        Array_Pattern{5,5}:=DINPUT(DiSensor55);
        
        FOR i FROM 1 TO 100 DO
            ! initialise path array to 0 
            FOR j FROM 1 TO 3 DO
                ! 
                Path_Array{i,j}:=0;
            ENDFOR
        ENDFOR

        FOR i FROM 1 TO 5 DO
            !9 rows 
            ivalue:=i MOD 2;
            IF ivalue=1 THEN
                FOR j FROM 1 TO 5 DO
                    ! 10 columns 
                    IF (Array_Pattern{i,j}=1) THEN
                        ! checking for 1st element
                        Path_Array{countit,1}:=i;
                        Path_Array{countit,2}:=j;
                        Path_Array{countit,3}:=0;
                        countit:=countit+1;
                    ELSE
                        Path_Array{countit,1}:=i;
                        Path_Array{countit,2}:=j;
                        Path_Array{countit,3}:=1;
                        countit:=countit+1;
                    ENDIF
                ENDFOR
            ELSE
                FOR j FROM 5 TO 1 DO
                    ! 10 columns 
                    IF (Array_Pattern{i,j}=1) THEN
                        ! checking for 1st element
                        Path_Array{countit,1}:=i;
                        Path_Array{countit,2}:=j;
                        Path_Array{countit,3}:=0;
                        countit:=countit+1;

                    ELSE
                        Path_Array{countit,1}:=i;
                        Path_Array{countit,2}:=j;
                        Path_Array{countit,3}:=1;
                        countit:=countit+1;

                    ENDIF

                ENDFOR

            ENDIF

        ENDFOR
        move_routine;
    ENDPROC

    PROC move_routine()
        MoveL Home_pos,v100,fine,MyTool\WObj:=wobj0;
        N:=countit-1;
        FOR countit FROM 1 TO N DO
            IF (Path_Array{countit,3}=0) THEN
                MoveL Offs(Start_pos,(Path_Array{countit,1}-1)*30,(Path_Array{countit,2}-1)*30,-15),v30,fine,MyTool\WObj:=wobj0;
            ELSE
                MoveL Offs(Start_pos,(Path_Array{countit,1}-1)*30,(Path_Array{countit,2}-1)*30,-25),v30,fine,MyTool\WObj:=wobj0;
            ENDIF
        ENDFOR
    ENDPROC
    PROC new_loop()

    ENDPROC
ENDMODULE
