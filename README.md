# Transform state
    tx, ty, tz = 0.0, 0.0, 0.0          # translation
    rx, ry = 25.0, -35.0                # a little initial rotation so you see 3D
    scale = 0.6                         # Step 5: smaller cube via glScalef

    clock = pygame.time.Clock()
    running = True
    while running:
        for event in pygame.event.get():
            if event.type == QUIT:
                running = False

            # --- Keyboard (Step 8) ---
            if event.type == KEYDOWN:
                if event.key == K_ESCAPE:
                    running = False

                # Translate (A/D, W/S, Q/E)
                if event.key == K_a:   tx -= 0.1     # left  (example from handout)
                if event.key == K_d:   tx += 0.1     # right
                if event.key == K_w:   ty += 0.1     # up
                if event.key == K_s:   ty -= 0.1     # down
                if event.key == K_q:   tz += 0.1     # toward camera
                if event.key == K_e:   tz -= 0.1     # away from camera

                # Rotate (arrow keys)
                if event.key == K_UP:    rx -= 5
                if event.key == K_DOWN:  rx += 5
                if event.key == K_LEFT:  ry -= 5
                if event.key == K_RIGHT: ry += 5

                # Scale ([ / ])
                if event.key == K_LEFTBRACKET:  scale = max(0.1, scale - 0.05)
                if event.key == K_RIGHTBRACKET: scale = min(3.0, scale + 0.05)

                # Reset
                if event.key == K_r:
                    tx, ty, tz = 0.0, 0.0, 0.0
                    rx, ry = 25.0, -35.0
                    scale = 0.6

        # --- Clear both color and depth buffers (Step 7) ---
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)

        glPushMatrix()
        # Apply transforms: translate -> rotate -> scale
        glTranslatef(tx, ty, tz)
        glRotatef(rx, 1, 0, 0)
        glRotatef(ry, 0, 1, 0)
        glScalef(scale, scale, scale)  # Step 5

        draw_cube()
        glPopMatrix()

        pygame.display.flip()
        clock.tick(60)

    pygame.quit()

if __name__ == "__main__":
    main()
