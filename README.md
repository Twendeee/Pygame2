# Q4 Lab 1 — Solid cube with 12 triangles, colors, scaling, depth test, and transforms
import pygame
from pygame.locals import *
from OpenGL.GL import *
from OpenGL.GLU import *

def setup_window(width=800, height=600):
    pygame.init()
    pygame.display.set_caption("Q4 Lab 1")
    pygame.display.set_mode((width, height), DOUBLEBUF | OPENGL)

    # --- Enable depth buffering (Step 7) ---
    glEnable(GL_DEPTH_TEST)

    # Projection
    glMatrixMode(GL_PROJECTION)
    glLoadIdentity()
    gluPerspective(45, width / float(height), 0.1, 50.0)

    # Modelview
    glMatrixMode(GL_MODELVIEW)
    glLoadIdentity()
    glTranslatef(0.0, 0.0, -7.0)  # Pull back the camera so cube is visible


# Vertex list from the handout (Step 3)
V = [
    ( 1,  1,  1),  # 0
    ( 1,  1, -1),  # 1
    ( 1, -1, -1),  # 2
    ( 1, -1,  1),  # 3
    (-1,  1,  1),  # 4
    (-1, -1, -1),  # 5
    (-1, -1,  1),  # 6
    (-1,  1, -1),  # 7
]

def v(i):
    """Emit one vertex by index i using glVertex3f."""
    glVertex3f(*V[i])

def draw_cube():
    glBegin(GL_TRIANGLES)

    # Right face (x = 1)
    glColor3f(1, 0, 0)  # Red
    v(0); v(1); v(3)
    v(1); v(2); v(3)

    # Left face (x = -1)
    glColor3f(0, 1, 0)  # Green
    v(4); v(6); v(7)
    v(6); v(5); v(7)

    # Top face (y = 1)
    glColor3f(0, 0, 1)  # Blue
    v(0); v(1); v(4)
    v(1); v(7); v(4)

    # Bottom face (y = -1)
    glColor3f(1, 1, 0)  # Yellow
    v(3); v(2); v(6)
    v(2); v(5); v(6)

    # Front face (z = 1)
    glColor3f(1, 0, 1)  # Magenta
    v(0); v(3); v(4)
    v(3); v(6); v(4)

    # Back face (z = -1)
    glColor3f(0, 1, 1)  # Cyan
    v(1); v(7); v(2)
    v(2); v(5); v(7)

    glEnd()


def main():
    setup_window()  # Initializes pygame + OpenGL

    # Initial transform state
    tx, ty, tz = 0.0, 0.0, 0.0
    rx, ry = 25.0, -35.0
    scale = 0.6  # Smaller cube

    clock = pygame.time.Clock()  # ✅ Works because pygame.init() already ran
    running = True

    while running:
        for event in pygame.event.get():
            if event.type == QUIT:
                running = False

            if event.type == KEYDOWN:
                if event.key == K_ESCAPE:
                    running = False

                # Translation
                if event.key == K_a: tx -= 0.1
                if event.key == K_d: tx += 0.1
                if event.key == K_w: ty += 0.1
                if event.key == K_s: ty -= 0.1
                if event.key == K_q: tz += 0.1
                if event.key == K_e: tz -= 0.1

                # Rotation
                if event.key == K_UP: rx -= 5
                if event.key == K_DOWN: rx += 5
                if event.key == K_LEFT: ry -= 5
                if event.key == K_RIGHT: ry += 5

                # Scale
                if event.key == K_LEFTBRACKET: scale = max(0.1, scale - 0.05)
                if event.key == K_RIGHTBRACKET: scale = min(3.0, scale + 0.05)

                # Reset
                if event.key == K_r:
                    tx, ty, tz = 0.0, 0.0, 0.0
                    rx, ry = 25.0, -35.0
                    scale = 0.6

        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)

        glPushMatrix()
        glTranslatef(tx, ty, tz)
        glRotatef(rx, 1, 0, 0)
        glRotatef(ry, 0, 1, 0)
        glScalef(scale, scale, scale)

        draw_cube()
        glPopMatrix()

        pygame.display.flip()
        clock.tick(60)

    pygame.quit()


if __name__ == "__main__":
    main()
