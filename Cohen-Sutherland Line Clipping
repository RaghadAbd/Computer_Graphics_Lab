#include <GL/glut.h>
int ww = 600, wh = 400;   
int first = 0;
int xi, yi, xf, yf;
const GLint LEFT_BIT = 1;     
const GLint RIGHT_BIT = 2;    
const GLint BOTTOM_BIT = 4;   
const GLint TOP_BIT = 8;     
void display()
{
	glClearColor(1.0, 1.0, 1.0, 1.0);
	glClear(GL_COLOR_BUFFER_BIT);
	glFlush();
}

typedef struct {
	GLfloat x, y;
} Point;

void init_graph(int argc, char** argv) {  
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glClearColor(1.0, 1.0, 1.0, 0.0);
	glPointSize(1.0f);
	glMatrixMode(GL_PROJECTION);
	glutMainLoop();
}
void swap_points(Point* p1, Point* p2) {
	Point t = *p1;
	*p1 = *p2;
	*p2 = t;
}

void swap_codes(GLint* x, GLint* y) {
	GLint t = *x;
	*x = *y;
	*y = t;
}

GLint inside(GLint code) {
	return !code;
}

GLint accept(GLint code1, GLint code2) {
	return !(code1 | code2);
}

GLint reject(GLint code1, GLint code2) {
	return code1 & code2;
}


GLint encode(Point p1, Point win_min, Point win_max) {
	GLint code = 0;

	if (p1.x < win_min.x) code |= LEFT_BIT;   
	if (p1.x > win_max.x) code |= RIGHT_BIT;  
	if (p1.y < win_min.y) code |= BOTTOM_BIT; 
	if (p1.y > win_max.y) code |= TOP_BIT;   
	return code;
}


void line_clip(Point p1, Point p2, Point win_min, Point win_max) {
	GLint code1, code2;
	GLint done = 0, plot_line1 = 0, plot_lines2 = 0;
	GLfloat m;
	m = (p2.y - p1.y) / (p2.x - p1.x);
	while (!done) {
		code1 = encode(p1, win_min, win_max);
		code2 = encode(p2, win_min, win_max);
		if (accept(code1, code2)) {
			done = 1;
			plot_line1 = 1;
		}
		else if (reject(code1, code2)) {
			done = 1;
			plot_lines2 = 1;
		}
		else {
			if (inside(code1)) {
				swap_points(&p1, &p2);
				swap_codes(&code1, &code2);
			}


			if (code1 & LEFT_BIT) {
				p1.y += (win_min.x - p1.x) * m;
				p1.x = win_min.x;
			}
			else if (code1 & RIGHT_BIT) {
				p1.y += (win_max.x - p1.x) * m;
				p1.x = win_max.x;
			}
			else if (code1 & BOTTOM_BIT) {
				if (p1.x != p2.x)
					p1.x += (win_min.y - p1.y) / m;
				p1.y = win_min.y;
			}
			else if (code1 & TOP_BIT) {
				if (p1.x != p2.x)
					p1.x += (win_max.y - p1.y) / m;
				p1.y = win_max.y;
			}
		}
	}

	if (plot_line1) {
		glColor3f(0, 1, 0);
		glLineWidth(2);
		glBegin(GL_LINES);
		glVertex2i((p1.x), (p1.y));
		glVertex2i((p2.x), (p2.y));
		glEnd();
		glFlush();
	}
	else if (plot_lines2) {
		glColor3f(1, 0, 0);
		glLineWidth(2);
		glBegin(GL_LINES);
		glVertex2i((p1.x), (p1.y));
		glVertex2i((p2.x), (p2.y));
		glEnd();
		glFlush();
	}
}

void draw_rectangle(Point rec_min, Point rec_max) {

	glColor3f(0, 0, 0);
	glBegin(GL_LINE_LOOP);
	glVertex2i((rec_min.x), (rec_min.y));
	glVertex2i((rec_min.x), (rec_max.y));
	glVertex2i((rec_max.x), (rec_max.y));
	glVertex2i((rec_max.x), (rec_min.y));
	glEnd();
	glFlush();
}
void R()
{
	Point win_min = { 100, 100 };
	Point win_max = { 370, 290 };
	draw_rectangle(win_min, win_max);
}
void drawLine(int x1, int y1, int x2, int y2)
{
	glClear(GL_COLOR_BUFFER_BIT);
	glLineWidth(2);
	Point win_min = { 100, 100 };
	Point win_max = { 370, 290 };
	draw_rectangle(win_min, win_max);
	Point p1 = { x1, y1 };
	Point p2 = { x2, y2 };
	glColor3f(1, 0, 0);
	glBegin(GL_LINES);
	glVertex2i((p1.x), (p1.y));
	glVertex2i((p2.x), (p2.y));
	glEnd();
	line_clip(p1, p2, win_min, win_max);
	glEnd();
	glFlush();
}
void mouse(int btn, int state, int x, int y)
{
	if (btn == GLUT_LEFT_BUTTON && state == GLUT_UP)
	{
		switch (first)
		{
		case 0:
			xi = x;
			yi = wh - y;
			R();
			first = 1;
			break;
		case 1:
			xf = x;
			yf = wh - y;
			R();
			drawLine(xi, yi, xf, yf);
			first = 0;
			break;
		}
	}
}


void myinit()
{
	glViewport(0, 0, ww, wh);
	glLoadIdentity();
	gluOrtho2D(0.0, (GLdouble)ww, 0.0, (GLdouble)wh);
	glMatrixMode(GL_MODELVIEW);
}
int main(int argc, char** argv)
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowSize(ww, wh);
	glutCreateWindow("Cohen Sutherland Algorithm");
	glutDisplayFunc(display);
	glutMouseFunc(mouse);
	myinit();
	glutMainLoop();
}

